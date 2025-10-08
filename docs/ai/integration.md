# AI Integration Documentation

## 🤖 AI System Overview

Drug Twin mengintegrasikan Google Gemini API untuk memberikan rekomendasi obat yang cerdas dan mendukung sistem chat AI. Sistem AI ini dirancang untuk membantu pengambilan keputusan klinis dengan menganalisis data pasien, riwayat medis, dan kondisi klinis terkini.

## 🏗️ AI Architecture

```
┌─────────────────────┐    ┌─────────────────────┐    ┌─────────────────────┐
│   Laravel Backend   │    │   Google Gemini     │    │   Custom ML Model   │
│   AI Service Layer  │◄──►│      API            │◄──►│   Drug Dosage       │
│                     │    │                     │    │   Recommendations   │
└─────────────────────┘    └─────────────────────┘    └─────────────────────┘
         │                            │                            │
         │                            │                            │
         ▼                            ▼                            ▼
┌─────────────────────┐    ┌─────────────────────┐    ┌─────────────────────┐
│   Database Layer    │    │   AI Response       │    │   Knowledge Base    │
│   Patient Data      │    │   Processing        │    │   Drug Information  │
│   Lab Results       │    │   & Validation      │    │   & Guidelines      │
└─────────────────────┘    └─────────────────────┘    └─────────────────────┘
```

## 🔧 Google Gemini API Integration

### API Configuration

#### Environment Variables
```bash
# .env
GEMINI_API_KEY=your_gemini_api_key_here
GEMINI_MODEL=gemini-1.5-pro
GEMINI_BASE_URL=https://generativelanguage.googleapis.com/v1beta
GEMINI_MAX_TOKENS=4096
GEMINI_TEMPERATURE=0.3
```

#### Laravel Service Configuration
```php
// config/gemini.php
<?php

return [
    'api_key' => env('GEMINI_API_KEY'),
    'model' => env('GEMINI_MODEL', 'gemini-1.5-pro'),
    'base_url' => env('GEMINI_BASE_URL', 'https://generativelanguage.googleapis.com/v1beta'),
    'max_tokens' => env('GEMINI_MAX_TOKENS', 4096),
    'temperature' => env('GEMINI_TEMPERATURE', 0.3),
    'safety_settings' => [
        [
            'category' => 'HARM_CATEGORY_MEDICAL',
            'threshold' => 'BLOCK_MEDIUM_AND_ABOVE'
        ]
    ]
];
```

### Core AI Service Class

#### GeminiService (`app/Services/GeminiService.php`)
```php
<?php

namespace App\Services;

use Illuminate\Support\Facades\Http;
use Illuminate\Support\Facades\Log;
use App\Exceptions\AIServiceException;

class GeminiService
{
    private $apiKey;
    private $baseUrl;
    private $model;
    private $maxTokens;
    private $temperature;

    public function __construct()
    {
        $this->apiKey = config('gemini.api_key');
        $this->baseUrl = config('gemini.base_url');
        $this->model = config('gemini.model');
        $this->maxTokens = config('gemini.max_tokens');
        $this->temperature = config('gemini.temperature');
    }

    public function generateContent(array $messages, array $options = []): array
    {
        try {
            $response = Http::withHeaders([
                'Content-Type' => 'application/json',
            ])->post("{$this->baseUrl}/models/{$this->model}:generateContent", [
                'key' => $this->apiKey,
                'contents' => $this->formatMessages($messages),
                'generationConfig' => [
                    'maxOutputTokens' => $options['max_tokens'] ?? $this->maxTokens,
                    'temperature' => $options['temperature'] ?? $this->temperature,
                ],
                'safetySettings' => config('gemini.safety_settings')
            ]);

            if (!$response->successful()) {
                throw new AIServiceException('Gemini API request failed: ' . $response->body());
            }

            return $this->parseResponse($response->json());
        } catch (\Exception $e) {
            Log::error('Gemini API Error: ' . $e->getMessage());
            throw new AIServiceException('AI service temporarily unavailable');
        }
    }

    private function formatMessages(array $messages): array
    {
        $formatted = [];
        foreach ($messages as $message) {
            $formatted[] = [
                'role' => $message['role'] === 'user' ? 'user' : 'model',
                'parts' => [
                    ['text' => $message['content']]
                ]
            ];
        }
        return $formatted;
    }

    private function parseResponse(array $response): array
    {
        $content = $response['candidates'][0]['content']['parts'][0]['text'] ?? '';
        $finishReason = $response['candidates'][0]['finishReason'] ?? 'UNKNOWN';
        
        return [
            'content' => $content,
            'finish_reason' => $finishReason,
            'usage' => $response['usageMetadata'] ?? null
        ];
    }
}
```

## 💊 Drug Recommendation System

### AI Recommendation Service

#### DrugRecommendationService (`app/Services/DrugRecommendationService.php`)
```php
<?php

namespace App\Services;

use App\Models\Patient;
use App\Models\Prescription;
use App\Models\LabResult;
use App\Models\Medication;
use App\Models\DrugDosage;
use Carbon\Carbon;

class DrugRecommendationService
{
    private $geminiService;
    private $confidenceThreshold;

    public function __construct(GeminiService $geminiService)
    {
        $this->geminiService = $geminiService;
        $this->confidenceThreshold = config('app.ai_confidence_threshold', 0.75);
    }

    public function generateRecommendation(Prescription $prescription): array
    {
        $patient = $prescription->patient;
        $medications = $prescription->items;
        
        // Gather patient data
        $patientData = $this->gatherPatientData($patient);
        
        // Create AI prompt
        $prompt = $this->buildRecommendationPrompt($patientData, $medications);
        
        // Get AI recommendation
        $aiResponse = $this->geminiService->generateContent([
            [
                'role' => 'user',
                'content' => $prompt
            ]
        ]);

        // Parse and validate response
        $recommendation = $this->parseRecommendationResponse($aiResponse['content']);
        
        // Calculate confidence score
        $confidence = $this->calculateConfidenceScore($recommendation, $patientData);
        
        // Generate warnings and alerts
        $warnings = $this->generateWarnings($patientData, $recommendation);
        
        return [
            'confidence_score' => $confidence,
            'recommendations' => $recommendation['medications'],
            'warnings' => $warnings,
            'drug_interactions' => $recommendation['interactions'],
            'contraindications' => $recommendation['contraindications'],
            'clinical_notes' => $recommendation['clinical_notes'],
            'ai_reasoning' => $recommendation['reasoning']
        ];
    }

    private function gatherPatientData(Patient $patient): array
    {
        $age = Carbon::parse($patient->birth_date)->age;
        
        // Get latest lab results
        $labResults = LabResult::where('patient_id', $patient->id)
            ->where('test_date', '>=', Carbon::now()->subDays(30))
            ->get()
            ->keyBy('test_name');

        // Calculate GFR if creatinine is available
        $gfr = $this->calculateGFR($patient, $labResults->get('Creatinine'));
        
        return [
            'demographics' => [
                'age' => $age,
                'gender' => $patient->gender,
                'weight' => $patient->weight,
                'height' => $patient->height,
                'bmi' => $patient->weight && $patient->height ? 
                    round($patient->weight / (($patient->height / 100) ** 2), 1) : null
            ],
            'medical_history' => $patient->medical_history ?? [],
            'allergies' => $patient->allergies ?? [],
            'lab_results' => [
                'gfr' => $gfr,
                'hba1c' => $labResults->get('HbA1c')?->test_value,
                'creatinine' => $labResults->get('Creatinine')?->test_value,
                'alt' => $labResults->get('ALT')?->test_value,
                'ast' => $labResults->get('AST')?->test_value,
                'hemoglobin' => $labResults->get('Hemoglobin')?->test_value,
            ],
            'current_medications' => $this->getCurrentMedications($patient),
            'comorbidities' => $this->extractComorbidities($patient->medical_history ?? [])
        ];
    }

    private function buildRecommendationPrompt(array $patientData, $medications): string
    {
        $prompt = "You are an expert clinical pharmacist AI assistant specializing in drug therapy optimization. ";
        $prompt .= "Analyze the following patient data and provide evidence-based medication recommendations.\n\n";
        
        $prompt .= "PATIENT DATA:\n";
        $prompt .= "Age: {$patientData['demographics']['age']} years\n";
        $prompt .= "Gender: {$patientData['demographics']['gender']}\n";
        $prompt .= "Weight: {$patientData['demographics']['weight']} kg\n";
        
        if ($patientData['lab_results']['gfr']) {
            $prompt .= "GFR: {$patientData['lab_results']['gfr']} mL/min/1.73m²\n";
        }
        
        if ($patientData['lab_results']['hba1c']) {
            $prompt .= "HbA1c: {$patientData['lab_results']['hba1c']}%\n";
        }
        
        $prompt .= "Medical History: " . implode(', ', $patientData['medical_history']) . "\n";
        $prompt .= "Allergies: " . implode(', ', $patientData['allergies']) . "\n\n";
        
        $prompt .= "PROPOSED MEDICATIONS:\n";
        foreach ($medications as $med) {
            $prompt .= "- {$med->medication->name}: {$med->dosage} {$med->frequency} for {$med->duration_days} days\n";
        }
        
        $prompt .= "\nPLEASE PROVIDE:\n";
        $prompt .= "1. Dosage appropriateness analysis\n";
        $prompt .= "2. Drug interaction warnings\n";
        $prompt .= "3. Contraindication alerts\n";
        $prompt .= "4. Kidney/liver function considerations\n";
        $prompt .= "5. Alternative recommendations if needed\n";
        $prompt .= "6. Monitoring parameters\n\n";
        
        $prompt .= "FORMAT: Respond in valid JSON format with the following structure:\n";
        $prompt .= "{\n";
        $prompt .= '  "medications": [{"name": "", "recommended_dose": "", "frequency": "", "adjustments": [], "reasoning": ""}],';
        $prompt .= '  "interactions": [{"severity": "", "description": ""}],';
        $prompt .= '  "contraindications": [],';
        $prompt .= '  "warnings": [{"type": "", "severity": "", "message": ""}],';
        $prompt .= '  "clinical_notes": "",';
        $prompt .= '  "reasoning": ""';
        $prompt .= "\n}";
        
        return $prompt;
    }

    private function parseRecommendationResponse(string $response): array
    {
        // Clean response and extract JSON
        $response = trim($response);
        
        // Remove markdown code blocks if present
        $response = preg_replace('/```json\s*/', '', $response);
        $response = preg_replace('/```\s*$/', '', $response);
        
        try {
            $decoded = json_decode($response, true);
            
            if (json_last_error() !== JSON_ERROR_NONE) {
                throw new \Exception('Invalid JSON response from AI');
            }
            
            return $decoded;
        } catch (\Exception $e) {
            Log::error('Failed to parse AI recommendation response: ' . $e->getMessage());
            
            // Return fallback response
            return [
                'medications' => [],
                'interactions' => [],
                'contraindications' => [],
                'warnings' => [
                    [
                        'type' => 'ai_error',
                        'severity' => 'high',
                        'message' => 'AI analysis incomplete. Manual review required.'
                    ]
                ],
                'clinical_notes' => 'AI recommendation parsing failed. Please review manually.',
                'reasoning' => 'System error occurred during AI analysis.'
            ];
        }
    }

    private function calculateConfidenceScore(array $recommendation, array $patientData): float
    {
        $score = 0.5; // Base score
        
        // Increase confidence if we have good lab data
        if ($patientData['lab_results']['gfr']) $score += 0.1;
        if ($patientData['lab_results']['hba1c']) $score += 0.1;
        
        // Decrease confidence if there are warnings
        $warningCount = count($recommendation['warnings'] ?? []);
        $score -= ($warningCount * 0.05);
        
        // Increase confidence if reasoning is detailed
        if (strlen($recommendation['reasoning'] ?? '') > 100) {
            $score += 0.1;
        }
        
        return max(0.0, min(1.0, $score));
    }

    private function generateWarnings(array $patientData, array $recommendation): array
    {
        $warnings = [];
        
        // Kidney function warnings
        if ($patientData['lab_results']['gfr'] && $patientData['lab_results']['gfr'] < 60) {
            $warnings[] = [
                'type' => 'kidney_function',
                'severity' => $patientData['lab_results']['gfr'] < 30 ? 'high' : 'medium',
                'message' => 'Reduced kidney function detected. Consider dose adjustment for renally cleared medications.'
            ];
        }
        
        // Age-related warnings
        if ($patientData['demographics']['age'] >= 65) {
            $warnings[] = [
                'type' => 'geriatric',
                'severity' => 'medium',
                'message' => 'Elderly patient. Consider starting with lower doses and monitor for increased sensitivity.'
            ];
        }
        
        // Drug allergy warnings
        foreach ($patientData['allergies'] as $allergy) {
            $warnings[] = [
                'type' => 'allergy',
                'severity' => 'high',
                'message' => "Patient has documented allergy to {$allergy}. Verify no cross-reactivity."
            ];
        }
        
        return array_merge($warnings, $recommendation['warnings'] ?? []);
    }

    private function calculateGFR(Patient $patient, ?LabResult $creatinine): ?float
    {
        if (!$creatinine || !$patient->weight) {
            return null;
        }
        
        $age = Carbon::parse($patient->birth_date)->age;
        $genderMultiplier = $patient->gender === 'female' ? 0.85 : 1.0;
        
        // Cockcroft-Gault equation
        $gfr = ((140 - $age) * $patient->weight * $genderMultiplier) / 
               (72 * $creatinine->test_value);
        
        return round($gfr, 1);
    }

    private function getCurrentMedications(Patient $patient): array
    {
        return Prescription::where('patient_id', $patient->id)
            ->where('status', 'approved')
            ->with('items.medication')
            ->get()
            ->flatMap(function ($prescription) {
                return $prescription->items->map(function ($item) {
                    return [
                        'name' => $item->medication->name,
                        'dosage' => $item->dosage,
                        'frequency' => $item->frequency
                    ];
                });
            })
            ->toArray();
    }

    private function extractComorbidities(array $medicalHistory): array
    {
        $commonConditions = [
            'diabetes' => ['diabetes', 'dm', 'diabetic'],
            'hypertension' => ['hypertension', 'htn', 'high blood pressure'],
            'heart_disease' => ['coronary', 'cardiac', 'heart disease', 'mi', 'myocardial'],
            'kidney_disease' => ['chronic kidney', 'ckd', 'renal', 'nephropathy'],
            'liver_disease' => ['hepatic', 'liver', 'cirrhosis', 'hepatitis']
        ];
        
        $comorbidities = [];
        
        foreach ($medicalHistory as $condition) {
            $condition = strtolower($condition);
            foreach ($commonConditions as $category => $keywords) {
                foreach ($keywords as $keyword) {
                    if (str_contains($condition, $keyword)) {
                        $comorbidities[] = $category;
                        break 2;
                    }
                }
            }
        }
        
        return array_unique($comorbidities);
    }
}
```

## 💬 AI Chat System

### Chat AI Service

#### ChatAIService (`app/Services/ChatAIService.php`)
```php
<?php

namespace App\Services;

use App\Models\User;
use App\Models\Patient;
use App\Models\ChatConversation;
use App\Models\ChatMessage;

class ChatAIService
{
    private $geminiService;

    public function __construct(GeminiService $geminiService)
    {
        $this->geminiService = $geminiService;
    }

    public function generateChatResponse(
        string $message, 
        User $user, 
        ?ChatConversation $conversation = null
    ): array {
        // Build context based on user role and conversation history
        $context = $this->buildChatContext($user, $conversation);
        
        // Create system prompt based on user role
        $systemPrompt = $this->getSystemPrompt($user->role);
        
        // Build conversation history
        $messages = [$systemPrompt];
        
        if ($conversation) {
            $recentMessages = $conversation->messages()
                ->orderBy('created_at', 'desc')
                ->limit(10)
                ->get()
                ->reverse();
                
            foreach ($recentMessages as $msg) {
                $messages[] = [
                    'role' => $msg->sender_type === 'ai' ? 'assistant' : 'user',
                    'content' => $msg->message
                ];
            }
        }
        
        // Add current message
        $messages[] = [
            'role' => 'user',
            'content' => $message
        ];
        
        // Get AI response
        $response = $this->geminiService->generateContent($messages);
        
        // Process response for safety and relevance
        $processedResponse = $this->processAIResponse($response['content'], $user->role);
        
        return [
            'response' => $processedResponse['message'],
            'confidence' => $processedResponse['confidence'],
            'sources' => $processedResponse['sources'],
            'follow_up_questions' => $processedResponse['follow_up_questions'],
            'requires_human_review' => $processedResponse['requires_human_review']
        ];
    }

    private function buildChatContext(User $user, ?ChatConversation $conversation): array
    {
        $context = [
            'user_role' => $user->role,
            'user_name' => $user->name,
            'timestamp' => now()->toDateTimeString()
        ];
        
        // Add patient-specific context
        if ($user->role === 'patient') {
            $patient = Patient::where('user_id', $user->id)->first();
            if ($patient) {
                $context['patient_data'] = [
                    'age' => $patient->birth_date ? 
                        \Carbon\Carbon::parse($patient->birth_date)->age : null,
                    'medical_history' => $patient->medical_history ?? [],
                    'allergies' => $patient->allergies ?? [],
                    'current_medications' => $this->getCurrentMedications($patient)
                ];
            }
        }
        
        // Add conversation context
        if ($conversation) {
            $context['conversation_type'] = $conversation->type;
            $context['participants'] = $this->getConversationParticipants($conversation);
        }
        
        return $context;
    }

    private function getSystemPrompt(string $userRole): array
    {
        $basePrompt = "You are DrugTwin AI, a helpful medical assistant specializing in medication management and drug information. ";
        $basePrompt .= "Always provide accurate, evidence-based information. ";
        $basePrompt .= "If you're unsure about medical advice, recommend consulting with healthcare providers. ";
        $basePrompt .= "Never diagnose conditions or recommend specific treatments without proper medical evaluation.\n\n";
        
        switch ($userRole) {
            case 'patient':
                $basePrompt .= "You are speaking with a patient. Use simple, clear language. ";
                $basePrompt .= "Focus on medication education, adherence support, and general health information. ";
                $basePrompt .= "Always emphasize the importance of following prescribed treatments and consulting healthcare providers for medical decisions.";
                break;
                
            case 'doctor':
                $basePrompt .= "You are assisting a doctor. You can provide detailed clinical information, ";
                $basePrompt .= "drug interactions, dosing guidelines, and evidence-based recommendations. ";
                $basePrompt .= "Reference clinical studies and guidelines when appropriate.";
                break;
                
            case 'pharmacist':
                $basePrompt .= "You are assisting a pharmacist. Focus on drug information, ";
                $basePrompt .= "pharmaceutical calculations, drug interactions, contraindications, ";
                $basePrompt .= "and pharmacy practice guidelines.";
                break;
                
            case 'admin':
                $basePrompt .= "You are assisting healthcare admin staff. Provide general information ";
                $basePrompt .= "about medications, hospital policies, and patient care coordination.";
                break;
        }
        
        return [
            'role' => 'system',
            'content' => $basePrompt
        ];
    }

    private function processAIResponse(string $response, string $userRole): array
    {
        // Basic safety filtering
        $safetyFilters = [
            '/\b(prescribe|diagnosis|diagnose)\b/i',
            '/\b(medical emergency|emergency)\b/i',
            '/\b(suicide|self-harm)\b/i'
        ];
        
        $requiresHumanReview = false;
        foreach ($safetyFilters as $filter) {
            if (preg_match($filter, $response)) {
                $requiresHumanReview = true;
                break;
            }
        }
        
        // Calculate confidence based on response characteristics
        $confidence = $this->calculateResponseConfidence($response);
        
        // Extract follow-up questions
        $followUpQuestions = $this->extractFollowUpQuestions($response, $userRole);
        
        // Add disclaimer for medical advice
        if ($userRole === 'patient' && !$requiresHumanReview) {
            $response .= "\n\n⚠️ This information is for educational purposes only. ";
            $response .= "Always consult your healthcare provider for medical advice.";
        }
        
        return [
            'message' => $response,
            'confidence' => $confidence,
            'sources' => ['medical_literature', 'drug_database'],
            'follow_up_questions' => $followUpQuestions,
            'requires_human_review' => $requiresHumanReview
        ];
    }

    private function calculateResponseConfidence(string $response): float
    {
        $confidence = 0.7; // Base confidence
        
        // Higher confidence for detailed responses
        if (strlen($response) > 200) $confidence += 0.1;
        
        // Lower confidence for uncertain language
        $uncertainWords = ['might', 'could', 'possibly', 'may', 'unsure'];
        foreach ($uncertainWords as $word) {
            if (stripos($response, $word) !== false) {
                $confidence -= 0.05;
            }
        }
        
        // Higher confidence for medical references
        if (stripos($response, 'study') !== false || 
            stripos($response, 'research') !== false) {
            $confidence += 0.1;
        }
        
        return max(0.1, min(0.95, $confidence));
    }

    private function extractFollowUpQuestions(string $response, string $userRole): array
    {
        $questions = [];
        
        switch ($userRole) {
            case 'patient':
                $questions = [
                    "Do you have any questions about your medication schedule?",
                    "Are you experiencing any side effects?",
                    "Would you like information about drug interactions?"
                ];
                break;
                
            case 'doctor':
                $questions = [
                    "Would you like dosing recommendations for specific patient populations?",
                    "Do you need information about drug monitoring parameters?",
                    "Would you like to see contraindication details?"
                ];
                break;
                
            case 'pharmacist':
                $questions = [
                    "Do you need pharmaceutical calculation assistance?",
                    "Would you like information about generic substitutions?",
                    "Do you need storage or stability information?"
                ];
                break;
        }
        
        // Randomly select 2-3 questions
        return array_slice($questions, 0, rand(2, 3));
    }
}
```

## 📊 AI Analytics and Monitoring

### AI Performance Tracking

#### AIAnalyticsService (`app/Services/AIAnalyticsService.php`)
```php
<?php

namespace App\Services;

use App\Models\Prescription;
use App\Models\AIRecommendation;
use Carbon\Carbon;

class AIAnalyticsService
{
    public function getPerformanceMetrics(string $period = '30days'): array
    {
        $startDate = $this->getPeriodStartDate($period);
        
        $totalRecommendations = AIRecommendation::where('created_at', '>=', $startDate)
            ->count();
            
        $approvedRecommendations = AIRecommendation::where('created_at', '>=', $startDate)
            ->where('doctor_action', 'approved')
            ->count();
            
        $averageConfidence = AIRecommendation::where('created_at', '>=', $startDate)
            ->avg('confidence_score');
            
        $interventionsPrevented = AIRecommendation::where('created_at', '>=', $startDate)
            ->whereJsonLength('warnings', '>', 0)
            ->count();
        
        return [
            'total_recommendations' => $totalRecommendations,
            'approval_rate' => $totalRecommendations > 0 ? 
                round(($approvedRecommendations / $totalRecommendations) * 100, 1) : 0,
            'average_confidence' => round($averageConfidence, 3),
            'interventions_prevented' => $interventionsPrevented,
            'accuracy_improvement' => $this->calculateAccuracyImprovement($startDate)
        ];
    }

    public function getTopWarningTypes(string $period = '30days'): array
    {
        $startDate = $this->getPeriodStartDate($period);
        
        $recommendations = AIRecommendation::where('created_at', '>=', $startDate)
            ->whereJsonLength('warnings', '>', 0)
            ->get();
            
        $warningCounts = [];
        
        foreach ($recommendations as $rec) {
            $warnings = json_decode($rec->warnings, true) ?? [];
            foreach ($warnings as $warning) {
                $type = $warning['type'] ?? 'unknown';
                $warningCounts[$type] = ($warningCounts[$type] ?? 0) + 1;
            }
        }
        
        arsort($warningCounts);
        
        return array_slice($warningCounts, 0, 10, true);
    }

    private function getPeriodStartDate(string $period): Carbon
    {
        switch ($period) {
            case '7days': return Carbon::now()->subDays(7);
            case '30days': return Carbon::now()->subDays(30);
            case '90days': return Carbon::now()->subDays(90);
            case '1year': return Carbon::now()->subYear();
            default: return Carbon::now()->subDays(30);
        }
    }

    private function calculateAccuracyImprovement(Carbon $startDate): float
    {
        // Compare current period with previous period
        $periodLength = Carbon::now()->diffInDays($startDate);
        $previousStart = $startDate->copy()->subDays($periodLength);
        
        $currentAccuracy = $this->getAccuracyForPeriod($startDate, Carbon::now());
        $previousAccuracy = $this->getAccuracyForPeriod($previousStart, $startDate);
        
        return $currentAccuracy - $previousAccuracy;
    }

    private function getAccuracyForPeriod(Carbon $start, Carbon $end): float
    {
        $total = AIRecommendation::whereBetween('created_at', [$start, $end])
            ->whereNotNull('doctor_action')
            ->count();
            
        $approved = AIRecommendation::whereBetween('created_at', [$start, $end])
            ->where('doctor_action', 'approved')
            ->count();
            
        return $total > 0 ? ($approved / $total) * 100 : 0;
    }
}
```

## 🛡️ AI Safety and Validation

### Safety Measures

#### AI Response Validation
```php
<?php

namespace App\Services;

class AISafetyService
{
    private $dangerousPatterns = [
        '/\b(increase|decrease) dose to \d+/i',
        '/\b(stop|discontinue) all medications?\b/i',
        '/\bmedicine.{0,20}(cure|heal) cancer\b/i',
        '/\b(guaranteed|promise) (cure|treatment)\b/i'
    ];
    
    private $warningPatterns = [
        '/\b(may|might|could) (cause|lead to) death\b/i',
        '/\b(emergency|urgent|immediate) medical attention\b/i',
        '/\b(severe|serious|life-threatening) (side effect|reaction)\b/i'
    ];

    public function validateResponse(string $response, string $context = ''): array
    {
        $validation = [
            'is_safe' => true,
            'confidence' => 1.0,
            'warnings' => [],
            'requires_review' => false
        ];

        // Check for dangerous patterns
        foreach ($this->dangerousPatterns as $pattern) {
            if (preg_match($pattern, $response)) {
                $validation['is_safe'] = false;
                $validation['requires_review'] = true;
                $validation['warnings'][] = 'Potentially harmful medical advice detected';
                break;
            }
        }

        // Check for warning patterns
        foreach ($this->warningPatterns as $pattern) {
            if (preg_match($pattern, $response)) {
                $validation['requires_review'] = true;
                $validation['warnings'][] = 'Medical emergency content detected';
                $validation['confidence'] *= 0.8;
            }
        }

        // Validate medical claims
        if ($this->containsMedicalClaims($response)) {
            $validation['confidence'] *= 0.9;
            $validation['warnings'][] = 'Contains medical claims requiring verification';
        }

        return $validation;
    }

    private function containsMedicalClaims(string $response): bool
    {
        $medicalClaimPatterns = [
            '/\b(cure|heal|treat) .{0,30}(disease|condition|illness)\b/i',
            '/\b(proven|clinically proven) to .{0,30}(cure|heal|treat)\b/i',
            '/\b(safe|effective) for all patients?\b/i'
        ];

        foreach ($medicalClaimPatterns as $pattern) {
            if (preg_match($pattern, $response)) {
                return true;
            }
        }

        return false;
    }
}
```

## 🔄 AI Model Training and Improvement

### Feedback Loop System

#### AIFeedbackService (`app/Services/AIFeedbackService.php`)
```php
<?php

namespace App\Services;

use App\Models\AIRecommendation;
use App\Models\AIFeedback;

class AIFeedbackService
{
    public function recordFeedback(
        int $recommendationId,
        string $userRole,
        int $userId,
        string $feedbackType,
        array $feedback
    ): void {
        AIFeedback::create([
            'ai_recommendation_id' => $recommendationId,
            'user_role' => $userRole,
            'user_id' => $userId,
            'feedback_type' => $feedbackType, // 'approval', 'rejection', 'modification'
            'feedback_data' => $feedback,
            'created_at' => now()
        ]);

        // Update recommendation status
        $recommendation = AIRecommendation::find($recommendationId);
        if ($recommendation) {
            $recommendation->update([
                'doctor_action' => $feedbackType,
                'doctor_feedback' => $feedback
            ]);
        }
    }

    public function getImprovementSuggestions(): array
    {
        // Analyze feedback patterns to suggest improvements
        $rejectionReasons = AIFeedback::where('feedback_type', 'rejection')
            ->where('created_at', '>=', now()->subDays(30))
            ->get()
            ->groupBy(function ($feedback) {
                return $feedback->feedback_data['reason'] ?? 'unknown';
            })
            ->map->count()
            ->sortDesc();

        $commonModifications = AIFeedback::where('feedback_type', 'modification')
            ->where('created_at', '>=', now()->subDays(30))
            ->get()
            ->groupBy(function ($feedback) {
                return $feedback->feedback_data['modification_type'] ?? 'unknown';
            })
            ->map->count()
            ->sortDesc();

        return [
            'most_common_rejections' => $rejectionReasons->take(5),
            'most_common_modifications' => $commonModifications->take(5),
            'recommendations' => $this->generateImprovementRecommendations(
                $rejectionReasons, 
                $commonModifications
            )
        ];
    }

    private function generateImprovementRecommendations(
        $rejections, 
        $modifications
    ): array {
        $recommendations = [];

        // Analyze rejection patterns
        foreach ($rejections->take(3) as $reason => $count) {
            switch ($reason) {
                case 'dosage_too_high':
                    $recommendations[] = "Consider implementing more conservative dosing algorithms";
                    break;
                case 'drug_interaction':
                    $recommendations[] = "Enhance drug interaction detection database";
                    break;
                case 'contraindication':
                    $recommendations[] = "Improve contraindication screening logic";
                    break;
            }
        }

        return $recommendations;
    }
}
```

## 📝 Integration Checklist

### Backend Setup
- [ ] Install Google Gemini API PHP client
- [ ] Configure environment variables
- [ ] Create AI service classes
- [ ] Implement safety validation
- [ ] Set up feedback collection
- [ ] Create analytics tracking

### Frontend Integration
- [ ] Create AI chat components
- [ ] Implement real-time messaging
- [ ] Build recommendation display
- [ ] Add confidence indicators
- [ ] Create feedback interfaces

### Testing
- [ ] Unit tests for AI services
- [ ] Integration tests with Gemini API
- [ ] Safety validation tests
- [ ] Performance benchmarks
- [ ] User acceptance testing

### Monitoring
- [ ] Set up AI performance dashboards
- [ ] Implement error tracking
- [ ] Create alert systems
- [ ] Monitor response times
- [ ] Track accuracy metrics

---
**Next Steps**: [Deployment Documentation](../deployment/guide.md)