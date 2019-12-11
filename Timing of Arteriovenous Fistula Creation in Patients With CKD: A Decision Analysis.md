## Timing of Arteriovenous Fistula Creation in Patients With CKD: A Decision Analysis

##### **H. C. Rayner, E. Imai, V. Kher**

### Definitions  
- **Monte Carlo Simulation**: broad class of computational algorithms that rely on repeated random sampling to obtain numerical results.  Their essential idea is using randomness so solve problems that might be deterministic in principle

### Background
- Approximately 23 million American adults have CKD, and 550,000 have end-stage kidney disease, and most of these patients are treated with hemodialysis
- The preferred vascular access for hemodialysis is an arteriovenous fistula (AVF) due to greater longevity and lower complication rates; however it may take several months and more than one procedure to establish a usable AVF
- If the AVF is created too late, it may not mature in time, and a central venous catheter (CVC) may be used.  However, CVC’s are associated with increased risk of morbidity and mortality
- Creating an AVF too early is also undesirable due to a small increase in risk of complications and wasting the limited lifetime
- Existing guidelines for AVF referral are inconsistent and based on expert opinion.  During the past decade, the KDOQI recommendations have varied from referral for AVF creation when HD is anticipated within 12 months, within 6 months, or when eGFR decreases to < 30 mL/min/1.73m^2
- In 2006, the Canadian Society of Nephrology guidelines suggested referral at eGFR of 15-20 mL/min/1.73m^2 in patients with progressive CKD
- Establishing clearer guidelines may improve incident AVF rates in HD patients and thereby positively affect patient outcomes 

### Methods
- Authors developed a Monte Carlo computer simulations model in C++ to determine the optimal timing of AVF referral in patients with CKD.  Authors evaluated two AVF referral strategies based on approaches suggested in recently published guidelines
  1. A “prep-window” strategy, in which referral occurs as soon as hemodialysis therapy initiation is anticipated within a specific time window (e.g next 12 months)
  2. An “eGFR-threshold” strategy in which patients are referred as soon as their eGFR falls below a specific threshold (e.g eGFR < 15 mL/min/1.73m^2) 

#### Parameters for CKD Progression
- Authors used linear regression modeling to estimate CKD progression parameters for 860 patients enrolled in a multidisciplinary kidney clinic
- Mean initial eGFR for our cohort was 3.8 mL/min/1.73m^2 and mean rate of eGFR decline was 2.78 mL/min/1.73m^2 per year

#### Patient Survival
- Patient survival was simulated according to whether the patient has non-dialysis-dependent CKD, is receiving hemodialysis with an AVF, or is receiving hemodialysis with a CVC.  Authors used survival data from the US Renal Data System (USRDS) to model pre-dialysis survival, and data from the USRDS and Ravani et al to model vascular access-specific survival for HD patients

#### AVF Referral Decision Making
- Authors assume that a patient’s eGFR is determined every 3 months for CKD stage 3, every 2 months for CKD stage 4, and every month for CKD stage 5
- After each simulated eGFR measurement, a decision is made to refer the patient for AVF creation or wait and reevaluate after the next eGFR determination 
- The timing of AVF referral is specified by the strategy being tested.  For the eGFR threshold strategy, AVF referral occurs when the simulated eGFR falls below the threshold value being tested
  - For the preparation-window strategy, AVF referral occurs when the anticipated HD therapy initiation date is within the time window being tested
- In reality, a nephrologists recommendation to initiate HD therapy is based on eGFR combined with other important factors, such as uremic symptoms.  However, these symptoms generally appear closer to the HD therapy initiation date and are not awaited before AVF referral 

#### Actual versus Estimated Hemodialysis Initiation Date
- The difference between the nephrologist’s estimated hemodialysis therapy initiation time and the actual HD therapy initiation time affects the degree to which an AVF will be ready before or after HD therapy initiation.  To account for the considerable inter patient variability in the actual eGFR at HD ther

#### Model Validation
- Authors compared survival curves of simulated patients who enter the clinic in CKD stages 3 and 4 with the Kaplan-Meier survival curves of their kidney clinic cohort

### Results

#### Incident Vascular Access Type and Unnecessary AVF creation
- As AVF referral occurred earlier (larger preparation window or higher eGFR threshold) the percentage of patients initiating therapy with a CVC decreased but the percentage with an unnecessary AVF increased 
- Relative differences between referral strategies were more pronounced for threshold policies with respect to the unnecessary AVF creation outcomes particular`
