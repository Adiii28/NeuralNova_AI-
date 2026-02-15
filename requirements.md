# Requirements Document

## Introduction

This document specifies requirements for an AI-powered, voice-first two-wheeler insurance platform designed for the Indian market (Bharat). The system enables multilingual voice-based insurance purchase and automated claims processing using computer vision, RAG-based decision making, and workflow automation. The platform aims to reduce claim processing time from days to under 1 hour while reducing operational costs by 35-40%.

## Glossary

- **Voice_Interface**: The multilingual voice interaction system using Deepgram STT, Llama 3.1 reasoning, and Kokoro TTS
- **Underwriting_Engine**: The AI-based system that evaluates risk and calculates premiums using violation history
- **Claims_Processor**: The automated system that processes claims using computer vision and RAG
- **Vision_Analyzer**: The computer vision component that detects damaged parts and assesses severity
- **RAG_System**: Retrieval-Augmented Generation system for policy clauses and garage tariffs
- **ATR**: Approval to Repair document issued after claim approval
- **Orchestrator**: The n8n-based workflow orchestration system
- **Fraud_Detector**: The system that detects fraudulent claims using metadata and anomaly scoring
- **PII_Masker**: The component that masks personally identifiable information before cloud processing
- **Policy_Document**: The digital insurance policy issued to customers
- **Claimable_Amount**: The calculated amount approved for repair based on policy and damage assessment
- **Garage_Network**: The network of approved repair garages
- **Violation_History**: Synthetic dataset of traffic violations used for underwriting

## Requirements

### Requirement 1: Multilingual Voice-Based Insurance Purchase

**User Story:** As a two-wheeler owner in India, I want to purchase insurance through voice interaction in my preferred language, so that I can easily buy insurance without language barriers or complex forms.

#### Acceptance Criteria

1. WHEN a user initiates insurance purchase, THE Voice_Interface SHALL support Hindi, English, Tamil, Telugu, Bengali, Marathi, Gujarati, Kannada, Malayalam, and Punjabi
2. WHEN a user speaks in their chosen language, THE Voice_Interface SHALL transcribe speech to text using Deepgram with 95% accuracy
3. WHEN transcribed text is received, THE Voice_Interface SHALL process intent using Llama 3.1 and generate appropriate responses
4. WHEN a response is generated, THE Voice_Interface SHALL convert text to speech using Kokoro TTS in the user's chosen language
5. WHEN voice interaction completes, THE Voice_Interface SHALL collect all required information for policy issuance including vehicle details, owner details, and coverage preferences

### Requirement 2: AI-Based Underwriting with Violation History

**User Story:** As an insurance provider, I want to assess risk using violation history data, so that I can offer personalized premiums based on actual risk profiles.

#### Acceptance Criteria

1. WHEN a policy application is received, THE Underwriting_Engine SHALL retrieve the applicant's violation history from the synthetic dataset
2. WHEN violation history is retrieved, THE Underwriting_Engine SHALL calculate a risk score based on violation type, frequency, and recency
3. WHEN the risk score is calculated, THE Underwriting_Engine SHALL apply risk-based pricing adjustments to the base premium
4. WHEN premium calculation is complete, THE Underwriting_Engine SHALL provide explainable reasoning for the premium amount using RAG_System
5. IF no violation history exists, THEN THE Underwriting_Engine SHALL apply standard base premium rates

### Requirement 3: Instant Digital Policy Issuance

**User Story:** As a customer, I want to receive my insurance policy immediately after purchase, so that I can have instant coverage and documentation.

#### Acceptance Criteria

1. WHEN payment is confirmed, THE System SHALL generate a Policy_Document within 30 seconds
2. WHEN a Policy_Document is generated, THE System SHALL include policy number, coverage details, premium amount, validity period, and terms and conditions
3. WHEN a Policy_Document is created, THE System SHALL deliver it via SMS, email, and WhatsApp to the customer
4. WHEN a Policy_Document is issued, THE System SHALL store it securely with encryption at rest
5. WHEN a customer requests policy retrieval, THE System SHALL provide access within 5 seconds

### Requirement 4: Guided Photo Upload for Claims

**User Story:** As a claimant, I want clear guidance on what photos to capture after an accident, so that I can submit complete documentation for faster claim processing.

#### Acceptance Criteria

1. WHEN a user initiates a claim, THE Voice_Interface SHALL guide the user to capture 8 specific photos: front view, rear view, left side, right side, damaged area close-up, vehicle registration plate, odometer reading, and accident scene overview
2. WHEN each photo is requested, THE Voice_Interface SHALL provide voice instructions in the user's language describing the required angle and content
3. WHEN a photo is uploaded, THE System SHALL validate image quality including resolution minimum 1080p, proper lighting, and focus
4. WHEN a photo fails validation, THE System SHALL request re-upload with specific feedback on the issue
5. WHEN all 8 photos are uploaded successfully, THE System SHALL proceed to damage assessment

### Requirement 5: Computer Vision Damage Assessment

**User Story:** As a claims processor, I want automated damage detection and severity assessment, so that claims can be evaluated quickly and consistently without manual inspection.

#### Acceptance Criteria

1. WHEN claim photos are received, THE Vision_Analyzer SHALL detect all visible damaged parts including body panels, lights, mirrors, wheels, and mechanical components
2. WHEN damaged parts are detected, THE Vision_Analyzer SHALL classify severity as minor, moderate, or severe for each part
3. WHEN severity is assessed, THE Vision_Analyzer SHALL estimate repair cost for each damaged part with 85% accuracy compared to actual garage estimates
4. WHEN multiple damaged parts are detected, THE Vision_Analyzer SHALL generate a comprehensive damage report listing all parts and their severity
5. IF the Vision_Analyzer cannot confidently assess damage with 80% confidence threshold, THEN THE System SHALL flag the claim for manual review

### Requirement 6: RAG-Based Policy and Tariff Retrieval

**User Story:** As a claims processor, I want automated retrieval of relevant policy clauses and garage tariffs, so that claim decisions are accurate and compliant with policy terms.

#### Acceptance Criteria

1. WHEN a damage report is generated, THE RAG_System SHALL retrieve applicable policy clauses based on damage type and policy coverage
2. WHEN policy clauses are retrieved, THE RAG_System SHALL retrieve current garage tariff rates for each damaged part from the approved Garage_Network
3. WHEN tariff rates are retrieved, THE RAG_System SHALL match detected parts to standardized tariff categories with 95% accuracy
4. WHEN retrieval is complete, THE RAG_System SHALL provide source citations for all retrieved clauses and tariffs
5. WHEN policy exclusions apply, THE RAG_System SHALL identify and flag non-covered damages with explanation

### Requirement 7: Automated Claimable Amount Calculation

**User Story:** As a claimant, I want to know the approved claim amount immediately, so that I can proceed with repairs without waiting for manual approval.

#### Acceptance Criteria

1. WHEN damage assessment and policy retrieval are complete, THE Claims_Processor SHALL calculate the Claimable_Amount by summing covered repair costs minus deductibles
2. WHEN calculating the Claimable_Amount, THE Claims_Processor SHALL apply policy limits including per-part limits and total claim limits
3. WHEN depreciation applies, THE Claims_Processor SHALL calculate and apply age-based depreciation to parts as per policy terms
4. WHEN the Claimable_Amount is calculated, THE Claims_Processor SHALL generate an itemized breakdown showing part name, tariff rate, depreciation, and approved amount
5. WHEN the calculation is complete, THE Claims_Processor SHALL provide explainable reasoning for the amount using RAG_System with policy clause references

### Requirement 8: Instant ATR Issuance

**User Story:** As a garage owner, I want to receive Approval to Repair authorization instantly, so that I can begin repairs immediately without waiting for paperwork.

#### Acceptance Criteria

1. WHEN the Claimable_Amount is approved, THE System SHALL generate an ATR document within 60 seconds
2. WHEN an ATR is generated, THE System SHALL include claim number, approved amount, itemized repair list, authorized garage details, and validity period of 15 days
3. WHEN an ATR is created, THE System SHALL digitally sign it for authenticity verification
4. WHEN an ATR is issued, THE System SHALL notify the claimant and assigned garage via SMS, email, and WhatsApp
5. WHEN a garage receives an ATR, THE System SHALL provide a verification mechanism to confirm ATR authenticity

### Requirement 9: Automated Garage Notification and Payment

**User Story:** As a garage owner, I want automated notification and guaranteed payment, so that I can provide services efficiently without payment delays.

#### Acceptance Criteria

1. WHEN an ATR is issued, THE System SHALL automatically assign the claim to the nearest approved garage from the Garage_Network based on claimant location
2. WHEN a garage is assigned, THE System SHALL send repair authorization with full claim details and approved amount
3. WHEN repairs are completed, THE System SHALL allow the garage to submit completion confirmation with final photos
4. WHEN completion is confirmed, THE System SHALL initiate payment to the garage within 24 hours
5. WHEN payment is processed, THE System SHALL send payment confirmation to both garage and claimant

### Requirement 10: Fraud Detection System

**User Story:** As an insurance provider, I want to detect potentially fraudulent claims automatically, so that I can prevent fraud losses while processing legitimate claims quickly.

#### Acceptance Criteria

1. WHEN a claim is submitted, THE Fraud_Detector SHALL analyze photo metadata including timestamp, GPS location, device information, and image manipulation indicators
2. WHEN metadata is analyzed, THE Fraud_Detector SHALL calculate an anomaly score based on patterns including claim frequency, damage consistency, and historical patterns
3. WHEN the anomaly score exceeds 70% threshold, THE Fraud_Detector SHALL flag the claim for manual investigation
4. WHEN a claim is flagged, THE Fraud_Detector SHALL provide specific reasons for the flag with supporting evidence
5. WHEN the anomaly score is below 70%, THE System SHALL proceed with automated processing

### Requirement 11: Privacy-Safe PII Masking

**User Story:** As a customer, I want my personal information protected, so that my privacy is maintained when data is processed in cloud services.

#### Acceptance Criteria

1. WHEN customer data is collected, THE PII_Masker SHALL identify personally identifiable information including name, phone number, email, address, and vehicle registration
2. WHEN PII is identified, THE PII_Masker SHALL mask or tokenize PII before sending data to cloud-based services including Deepgram
3. WHEN data is masked, THE PII_Masker SHALL maintain a secure mapping to unmask data when needed for policy issuance or communication
4. WHEN data is stored, THE System SHALL encrypt PII at rest using AES-256 encryption
5. WHEN data is transmitted, THE System SHALL use TLS 1.3 for all network communications

### Requirement 12: Workflow Orchestration

**User Story:** As a system administrator, I want centralized workflow orchestration, so that I can manage, monitor, and modify business processes efficiently.

#### Acceptance Criteria

1. WHEN any business process is triggered, THE Orchestrator SHALL coordinate all system components using n8n workflows
2. WHEN a workflow executes, THE Orchestrator SHALL log all steps including timestamps, inputs, outputs, and errors
3. WHEN a workflow fails, THE Orchestrator SHALL implement retry logic with exponential backoff up to 3 attempts
4. WHEN retries are exhausted, THE Orchestrator SHALL escalate to manual intervention queue with full context
5. WHEN workflows are modified, THE Orchestrator SHALL support version control and rollback capabilities

### Requirement 13: Explainable Decision Making

**User Story:** As a customer, I want to understand why my premium or claim amount was calculated a certain way, so that I can trust the system's decisions.

#### Acceptance Criteria

1. WHEN a premium is calculated, THE System SHALL provide explanation including risk factors, violation impact, and pricing components
2. WHEN a claim amount is calculated, THE System SHALL provide explanation including covered parts, depreciation applied, and policy clause references
3. WHEN a claim is denied or partially approved, THE System SHALL provide specific policy clauses and reasons for the decision
4. WHEN an explanation is generated, THE RAG_System SHALL cite specific sources including policy sections and tariff references
5. WHEN a customer requests clarification, THE Voice_Interface SHALL provide additional explanation in conversational language

### Requirement 14: Regulatory Compliance Reporting

**User Story:** As a compliance officer, I want automated regulatory reporting, so that I can meet IRDAI requirements without manual data compilation.

#### Acceptance Criteria

1. WHEN the reporting period ends, THE System SHALL automatically generate required IRDAI reports including policy issuance statistics, claims statistics, and settlement ratios
2. WHEN reports are generated, THE System SHALL include all mandatory fields as per IRDAI guidelines
3. WHEN reports are created, THE System SHALL validate data completeness and accuracy before submission
4. WHEN validation passes, THE System SHALL format reports according to IRDAI specifications
5. WHEN reports are ready, THE System SHALL notify compliance officers and provide secure download access

### Requirement 15: Performance and Automation Targets

**User Story:** As a business owner, I want to achieve operational efficiency targets, so that I can reduce costs while improving customer experience.

#### Acceptance Criteria

1. WHEN a claim is submitted with all required information, THE System SHALL complete processing from submission to ATR issuance within 1 hour for 95% of claims
2. WHEN measuring automation rate, THE System SHALL achieve 90% workflow automation with less than 10% of claims requiring manual intervention
3. WHEN measuring cost efficiency, THE System SHALL reduce operational cost per claim by 35-40% compared to manual processing baseline
4. WHEN measuring accuracy, THE Vision_Analyzer SHALL achieve 85% accuracy in damage cost estimation compared to actual garage bills
5. WHEN measuring customer satisfaction, THE Voice_Interface SHALL achieve 90% successful task completion rate without human agent escalation

### Requirement 16: System Availability and Reliability

**User Story:** As a customer, I want the system to be available when I need it, so that I can purchase insurance or file claims without service interruptions.

#### Acceptance Criteria

1. THE System SHALL maintain 99.5% uptime measured monthly excluding planned maintenance windows
2. WHEN planned maintenance is required, THE System SHALL provide 48-hour advance notice to users
3. WHEN system load increases, THE System SHALL scale automatically to handle up to 10,000 concurrent voice sessions
4. WHEN a component fails, THE System SHALL implement graceful degradation allowing core functions to continue
5. WHEN system errors occur, THE System SHALL log errors with full context for debugging and recovery

### Requirement 17: Data Retention and Audit Trail

**User Story:** As a compliance officer, I want complete audit trails of all transactions, so that I can investigate disputes and meet regulatory requirements.

#### Acceptance Criteria

1. WHEN any transaction occurs, THE System SHALL create an immutable audit log entry with timestamp, user, action, and data changes
2. WHEN a policy is issued, THE System SHALL retain all application data, underwriting decisions, and policy documents for 7 years
3. WHEN a claim is processed, THE System SHALL retain all photos, damage reports, calculations, and communications for 7 years
4. WHEN audit logs are queried, THE System SHALL provide search and filter capabilities by date, user, claim number, and policy number
5. WHEN data retention period expires, THE System SHALL archive data to cold storage with retrieval capability within 24 hours

### Requirement 18: Local LLM Processing

**User Story:** As a system architect, I want to use locally hosted Llama 3.1 for reasoning, so that I can reduce cloud costs and maintain data privacy.

#### Acceptance Criteria

1. WHEN voice input is transcribed, THE System SHALL process reasoning and intent detection using locally hosted Llama 3.1 model
2. WHEN the local LLM processes requests, THE System SHALL achieve response latency under 2 seconds for 95% of requests
3. WHEN the local LLM is unavailable, THE System SHALL fail gracefully and queue requests for retry
4. WHEN model updates are available, THE System SHALL support hot-swapping of LLM models without downtime
5. WHEN processing sensitive data, THE System SHALL ensure all LLM processing occurs on-premises without external API calls
