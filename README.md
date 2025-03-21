# Organ_transplant

https://physionet.org/content/orchid/2.0.0/

Abstract
There are well-documented inefficiencies and inequities in the current system of deceased donor organ transplantation. While much prior research has focused on designing better allocation systems to distribute donated organs, more can be done to study organ procurement, the process by which organs are recovered from deceased donors. Improvements to organ procurement can greatly increase the supply of a scarce public resource, improving health outcomes for the more than 100,000 patients currently waiting for an organ transplant. Such changes will also impact health equity, as a disproportionate number of waitlisted patients come from marginalized communities. So far, research into organ procurement has been hamstrung by a lack of transparent data. To address this pressing issue, we have collected the first ever multi-center dataset on organ procurement, covering ten years of clinical, financial, and administrative information from six organ procurement organizations (OPOs). This novel dataset can be used to identify opportunities to improve current practice, build tools to help OPOs make better decisions, and recommend policy changes to increase procurement.

Background
Organ transplantation is a life-saving procedure for patients with advanced or end-stage diseases. While a significant amount of research has focused on the best way to allocate organs once donated [1-3], more can be done to examine the process by which organs are procured from deceased donors. This process is managed regionally by 56 organ procurement organizations (OPOs), federal contractors which are granted monopoly territories–known as donation service areas (DSAs)–by the Centers for Medicare & Medicaid Services (CMS). OPO performance across the country is highly variable, with some OPOs procuring only a fraction of all transplant-viable organs [4]. Shortcomings in procurement disproportionately impact patients who are Black, Indigenous, or people of color (BIPOC), as these communities have higher rates of diseases that necessitate transplantation [5]. Improving organ procurement is an urgent issue of public health and health equity; every year, over 10,000 patients die waiting for a transplant [5], while thousands of potentially transplantable organs go unrecovered [4].

Thus far, research into procurement performance has been severely limited by a lack of transparent data. Most current research on organ procurement is either anecdotal or based on aggregate metrics that OPOs self-report every few years. In collaboration with the Federation of American Scientists (FAS) and six OPOs, we have obtained the first ever multi-centre dataset on organ procurement. This novel, granular, and open health dataset will allow researchers to analyze organ procurement decisions, find inefficiencies in practice, and recommend actionable changes through data analyses and healthcare algorithms [6].

Methods
The described data were captured in the health information technology (IT) systems used by six OPOs routinely during the process of procurement between January 1, 2015 and December 31, 2021†. Data was transferred in the form of SQL database dumps, extracted and curated using SQL and Python, and released in the form of a comma-separated values (CSV) file. The dataset covers the following number of potential donors for each OPO:

OPO 1: 32,148
OPO 2: 16,144
OPO 3: 12,516
OPO 4: 33,641
OPO 5: 15,738
OPO 6: 22,914
The dataset is organized around the typical process of organ procurement at an OPO. There are six key steps in the procurement process: referral, evaluation, approach, authorization, procurement, and transplant. When a patient admitted to a hospital is near death, the hospital refers them to the local OPO for organ donation ("Referral"). The OPO evaluates each referral, and makes an initial assessment on the patient's suitability for organ donation ("Evaluation"). If the referred patient is deemed to be medically suitable, an OPO representative approaches the patient's next-of-kin (NOK) to obtain their consent for donation ("Approach"). If consent is obtained ("Authorization"), the OPO coordinates the process of procuring transplant-viable organs from the deceased patient ("Procurement"). They then offer each procured organ to patients on the national transplant waitlist in a priority order determined by the Organ Procurement and Transplantation Network (OPTN). The organ is allocated to the highest ranked patient whose transplant surgeon accepts the offer. Finally, the OPO coordinates the logistics of transporting the organ to the patient’s transplant center, and the transplant proceeds ("Transplant").

All data was de-identified in accordance with Health Insurance Portability and Accountability Act (HIPAA) standards using structured data cleansing and date shifting. This process required the removal of all eighteen of the identifying data elements listed in HIPAA, including patient name, address, and dates. Patients over the age of 89 had their exact age obscured and appear in the dataset with an age of 100.  Dates were shifted into the future by a random offset for each individual patient. Note that the date-shifting preserved intervals (e.g., the time between death and OPO approach) for each patient.

†The data for OPOs 1, 2, and 4 does not cover the entire study period. OPO 1 data covers Jan 1, 2015 - Nov 23, 2021, OPO 2 data covers Jan 1, 2018 - December 31, 2021, and OPO 4 data covers  Jan 1, 2015 - December 13, 2021.

Data Description
ORCHID consists of ten tables, each of which is provided as a comma separated value (CSV) file. Tables are linked via a unique patient identifier, PatientID. Each table falls into one of three categories: OPO Referrals, OPO Events, and OPO Deaths. We describe each of these in further detail below. Note that all timestamps in the data were shifted into the future by a random date offset for each individual patient. The date-shifting procedure preserved all intervals for each patient. Time of day was also preserved.

OPO Referrals
referrals.csv documents each patient referred to an OPO and contains demographics, cause-of-death, and procurement outcomes. The table covers 133,101 deceased donor referrals and 8,972 organ donations across 13 states. The table is unique at the patient level (identified by PatientID). We only include data on patients who were mechanically vented (a pre-requisite for organ donation) and referred for organ donation (vs. tissue or eye donation). The table contains three key types of information:

Referral information: patient-level data on each referral received by the OPO, including demographics and cause of death
Process data: timestamps that capture every action taken by an OPO representative in relation to a referral, including next-of-kin approach, authorization, and procurement. The table also includes time stamps for time of death (asystole and brain death, if applicable)
Outcomes: Binary indicators for if the patient was approached, authorized, or procured. If procured, the outcome of each organ recovered from the patient.
The data is organized as a CSV with 38 columns. Each of these is described in detail in the data dictionary, referrals_dictionary.html. Note that the data for OPOs 1, 2, and 4 does not cover the entire study period. OPO 1 data covers Jan 1, 2015 - Nov 23, 2021, OPO 2 data covers Jan 1, 2018 -December 31, 2021, and OPO 4 data covers Jan 1, 2015 - December 13, 2021.

OPO Events
The eight OPO Events tables describe events charted by OPO staff during a referred patient’s hospital stay and the subsequent procurement process. These can be linked to the OPO Referrals data using the identifier PatientID. Unless specified below, time_event notes the time of the charted event (e.g., time of lab test), shifted on a patient-level for de-identification purposes. The eight tables are:

ChemistryEvents: Laboratory test results for blood chemistry. Includes several donation-relevant tests, incuding kidney panel, liver function tests, and electrolyte panel.
CBCEvents: Test results from complete blood count (CBC) with differential.
ABGEvents: Arterial blood gas (ABG) test results. Includes relevant measurements (e.g., partial pressure of oxygen) along with ventilator setting at time of measurement.
SerologyEvents: Serological testing results. Records the presence (or absence) of donation-relevant antigens and antibodies (e.g., HIV, hepatitis C, etc.).
CultureEvents: Results of infection culture tests for blood, urine, and other body fluids.
HemoEvents: Hemodynamics (e.g., min/max/average blood pressure) over the time range defined by time_event_start and time_event_end. Note that if these timestamps are the same, the given observation reflects a point measurement (e.g., heart rate at a given point in time).
FluidBalanceEvents: Fluid balance (e.g. fluid intake, urine output) over the time range defined by time_event_start and time_event_end.
(Future release) NoteEvents: Free-text notes made by OPO staff during the process of procurement. These notes are currently being de-identified, and will be made available in a future release.
OPO Deaths
Each OPO's data only captures patients that were referred to the OPO by hospitals in their DSA. This process can vary by OPO, as well as within the same OPO by year and geography. As a result, the total number of referrals does not represent a consistent denominator over which to measure performance. To address this inconsistency, we provide calc_deaths.csv, which contains yearly estimates of the Cause, Age, and Location Consistent (CALC) deaths in each OPO's DSA. CALC deaths represent a more consistent metric that will be used as the denominator in calculating an OPO's donation rate under the new OPO Final Rule [7].

We calculate DSA-level CALC estimates from county level death statistics, obtained using the CDC WONDER tool [8]. Note that WONDER censors the exact number of deaths in counties that have fewer than 10 deaths. We provide a lower bound on CALC deaths (calc_deaths_lb) by assuming that each of these counties had exactly 1 death, an upper bound by assuming that each of these counties had exactly 9 deaths (calc_deaths_ub), and a median estimate by assuming that each of these counties had exactly 4 deaths (calc_deaths). Note that WONDER does not capture deaths of nonresidents (e.g. nonresident aliens, nationals living abroad), and so may underestimate CALC deaths in some counties.

Usage Notes
This dataset can be used to recommend actionable changes to the organ procurement process to improve both efficiency and equity. Improvements to organ procurement can greatly increase the supply of a scarce public resource, improving health outcomes for the more than 100,000 patients currently waiting for an organ transplant. Such changes will also impact health equity, as a disproportionate number of waitlisted patients come from marginalized communities. Users of the data should be aware that these data were not collected specifically for analysis, but routinely in the process of procurement. The provided data reflect the idiosyncrasies of procurement practice. The data may contain implausible values due to the archival process. Note that procurement practice may differ by OPO. Researchers should follow best practices while analyzing these data.

Release Notes
Version 2.0.0: Second release. Noteworthy changes:

Added seven OPO Events tables
Renamed:
opd.csv to referrals.csv
data_description.html to referrals_dictionary.html
Adjusted time stamps for three OPOs to reflect local time
Version 1.0.0: First release.

Ethics
The data transfer, storage, analysis, and release has been approved by COUHES, the Institutional Review Board at MIT (protocol #: 2201000540A001). A key goal of this data release is to improve equity in organ transplantation by increasing the number of transplant opportunities for patients from marginalized communities. Use of these data should follow the best practices of responsible big data research as outlined by the NSF [9].

Acknowledgements
We thank the six organ procurement organizations for their leadership and cooperation to drive improvements in their field through open data. We also would like to acknowledge key contributions made by data and business intelligence leaders at each OPO; FAS and Organize; Ari Perez, Teja Naraparaju, and Chengjie Yin for their data expertise; published researchers in this space including Brianna Doby BA and Ray Lynch MD; and philanthropic support for opening data in patients’ interests from Arnold Ventures, Lyda Hill Philanthropies, and Schmidt Futures.

Conflicts of Interest
The authors have no conflicts of interest to declare.

References
Su, X. & Zenios, S. A. Recipient Choice Can Address the Efficiency-Equity Trade-off in Kidney Transplantation: A Mechanism Design Model. Manage. Sci. 52, 1647–1660 (2006).
Agarwal, N., Ashlagi, I., Rees, M., Somaini, P. & Waldinger, D. Equilibrium Allocations under Alternative Waitlist Designs: Evidence from Deceased Donor Kidneys Kidneys. Econometrica 89, 37–76 (2021).
Berrevoets, J., Jordon, J., Bica, I., & van der Schaar, M. OrganITE: Optimal transplant donor organ offering using an individual treatment effect. Advances in neural information processing systems, 33, 20037-20050 (2020).
Rosenberg, P., Ciccarone, M., Seeman, B., Washer, D., Martin, G., Tse, J., & Lezama, E. Transforming Organ Donation in America. Bridgespan (2020).
Based on OPTN data as of January 16, 2023.
Alberto IR, Alberto NR, Ghosh AK, Jain B, Jayakumar S, Martinez-Martin N, McCague N, Moukheiber D, Moukheiber L, Moukheiber M, Moukheiber S. The impact of commercial health datasets on medical research and health-care algorithms. The Lancet Digital Health. 2023 May 1;5(5):e288-94.
Organ Procurement Organizations. Conditions for Coverage: Revisions to the Outcome Measure Requirements for Organ Procurement Organizations; Final rule. In. 42 CFR Part 486, Washington, DC, 2020.
Centers for Disease Control and Prevention, National Center for Health Statistics. Underlying Cause of Death 1999–2020 on CDC WONDER Online Database. http://wonder.cdc.gov/ucd-icd10.html, released in 2021. Accessed June 17, 2022.
Zook, M., Barocas, S., Boyd, D., Crawford, K., Keller, E., Gangadharan, S. P., ... & Pasquale, F. Ten simple rules for responsible big data research. PLoS computational biology, 13(3), e1005399 (2017).
Share
    
Access
Access Policy:
Only registered users who sign the specified data use agreement can access the files.

License (for files):
PhysioNet Restricted Health Data License 1.5.0

Data Use Agreement:
PhysioNet Restricted Health Data Use Agreement 1.5.0

Discovery
DOI (version 2.0.0):
https://doi.org/10.13026/w10c-6g08

DOI (latest version):
https://doi.org/10.13026/b1c0-3506

Topics:
organ procurement organizations organ transplantation

Corresponding Author
You must be logged in to view the contact information.
Versions
1.0.0 - July 25, 2023
2.0.0 - June 27, 2024
Files
This is a restricted-access resource. To access the files, you must fulfill all of the following requirements:
sign the data use agreement for the project
