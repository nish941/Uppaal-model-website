Scholarship Application Portal - UPPAAL Model 

📋 Project Overview
The Scholarship Application Portal is a formal model designed to simulate interactions between users and a scholarship portal system. The model ensures secure authentication, prevents multiple scholarship applications per session, and handles interactions with external components including credential verification, scholarship communication, and transcript information systems.

🏗️ Key Components
1. Website Model
Handles user interactions including:

User login and logout

Homepage navigation

Scholarship application process

Separation between Merit Scholarship and Merit-Cum-Need Scholarship

Prevention of multiple scholarship applications per session

2. CredentialChecker Model
Authenticates users based on unique IDs and passwords

Tracks authenticated users globally to prevent duplicate logins

States: Waiting, Verifying

Uses integer credentials for modeling simplicity

3. ScholarshipComm Model
Evaluates scholarship eligibility based on CGPA thresholds:

MCN Scholarship: Minimum CGPA ≥ 6

Merit Scholarship: Minimum CGPA ≥ 7

Processes scholarship requests and communicates decisions

Uses default CGPA = 8 for verification

Synchronizes with website to prevent multiple applications

Automatically redirects to idle/waiting after processing

4. TranscriptInfo Model
Handles requests for academic transcripts and personal information

Generates data based on user information from academic server

Includes fallback state for missing information

5. User Model
Simple model for user interactions with the website

⚙️ Technical Implementation
System Declarations
uppaal
// System declarations
Process = Website();
system Process, TranscriptInfo, ScholarshipComm, CredentialChecker;

// Channels
chan scholarshipDecision, scholarshipApplyMerit, transcriptRequest, 
     personalInfoRequest, loginRequest, loginResponse, logoutRequest, 
     scholarshipApplyMCN;

// Global variables
int scholarship_approved = 0;
✅ Verified Properties
Liveness Properties
Scholarship Approval Path: E<> ScholarshipComm.Approved ✓ TRUE

Authentication Success Path: E<> CredentialChecker.Success ✓ TRUE

State Consistency: A<> CredentialChecker.Verifying imply CredentialChecker.Waiting ✓ TRUE

Application Completion: E<> ScholarshipComm.Idle imply ScholarshipComm.scholarshipApplied ✓ TRUE

Login Verification: Website.login --> CredentialChecker.Verifying ✓ TRUE

Decision Path: E<> (ScholarshipComm.Approved || ScholarshipComm.Rejected) ✓ TRUE

Safety Properties
Deadlock Freedom: A[] !deadlock ✓ TRUE

Application State: E[] ScholarshipComm.EvaluatingMerit imply ScholarshipComm.scholarshipApplied ✓ TRUE

Verification State: A[] CredentialChecker.Verifying imply CredentialChecker.Waiting ✗ FALSE

Persistent Application: E[] ScholarshipComm.scholarshipApplied ✗ FALSE

Authentication Consistency: E[] CredentialChecker.Verifying imply CredentialChecker.is_authenticated ✓ TRUE

Authentication Outcome: E[] CredentialChecker.Verifying imply (!CredentialChecker.is_authenticated || CredentialChecker.is_authenticated) ✓ TRUE

🧪 Assumptions
Each user has a unique ID and valid credentials

Authentication server verifies all credentials

No support for multiple device sessions per user

State synchronization ensures consistent user actions

Immediate server responses (no delays modeled)

Only one user active at a time

Default values used for modeling:

Email, password, CGPA = 8

Transcript and personal information exist unless specified otherwise

No visual output (pure state transition model)

🚀 How to Run
Prerequisites
UPPAAL 4.0 or higher

XML model file

Steps
Import the XML model file into UPPAAL

Ensure system declarations are set as specified above

Verify properties in the "Verifier" tab

Simulate the model to trace state transitions

Model Files
scholarship_portal.xml - Main UPPAAL model

(Optional test cases with name, email, password, CGPA)

📈 Model Features
Security
Unique user authentication

Prevention of duplicate logins

Session management

Scholarship Processing
Separate merit and merit-cum-need pathways

CGPA-based eligibility checks

Single application per session restriction

Data Management
Transcript information retrieval

Personal data access

Fallback mechanisms for missing data

🔍 Verification Results
The model successfully verifies:

Safety: No deadlocks, consistent state transitions

Liveness: Critical states are reachable

Functional Correctness: Proper workflow from login to scholarship decision

📝 Conclusion
This UPPAAL model effectively simulates a Scholarship Application Portal with robust security and workflow management. The modular design allows for future extensions including:

Multiple simultaneous user sessions

Real-world delay integration

Additional scholarship types

Enhanced security features

The verification of key safety and liveness properties ensures the model's correctness and reliability for real-world implementation.

📄 License
Educational project for BITS Pilani course CS F214
