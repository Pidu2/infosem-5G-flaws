Same researchers as 5GReasoner, 4G and 5G: (side channel attack)
* If TMSI changed infrequently (deployment weakness): "An attacker places multiple phone calls to the victim device in a short period of time and sniffs the paging messages.  If the most frequent TMSI among the paging messages appears frequently enough, then the attacker concludes that the victim device is present" (no weakness of this paper)
* ToRPEDO (...able to verify whether a victim device is present in a geographical cell with less than 10 calls, even under the assumption that the TMSI changes after each call. Furthermore, in the process, the attacker learns exactly when a device wakes up to check for paging messages and 7 bits of information of the device’s International Mobile Subscriber Identity (IMSI). How exactly -> see paper. Using PFI side channel info...)
  * attacker can hijack paging channel
    * DoS
    * fabricated emergency messages
  * location (cell)
  * detect connection status (idle/connected)
  * basis for PIERCER and IMSI-Cracking
* (PIERCER
  * associate victims phone nbr with it's IMSI (-> targeted user tracking)) ---> PIERCER only 4G
* IMSI-Cracking (need to know phone nbr. IMSI 49bit. leading 18 can be obtained from phone nbr using lookup services. with torpedo, trailing 7 bits are exposed, leaving 24bits -> brute force)
  * get IMSI through brute force

5GReasoner: (11 new and 5 old from 4g)
* NAS Layer:
  * NAS Counter Reset
Reset Counters, which gets UE and AMF out of sync and makes them derive a different key. This in turn causes the UE to reestablish the connection with the network. This can be redone multiple times -> DoS & Battery depletion. Also possible to replay same user plane packets over and over again to cause over billing. Needed: fake base station. Also relay can drop packets without getting detected.
    * battery depletion
    * DoS
    * over billing
    * packet dropping without detection
  * Uplink NAS Counter Desynchronization:
Neither UE nor AMF check how many times some message fails. -> Attacker inserts messages, which UE declines. UE increments Counter, at 2^8 it increments the uplink overflow counter. after that, any uplink message violates sanity check -> prolonged DoS until connection drop&reauthenticate or tracking_area_update by AMF.   
    * prolonged DoS
  * Exposing NAS Sequence Number
Sequence Numbers are included plain text
    * monitor activity & service usage
  * Neutralizing TMSI Refreshement
MitM and attacker knows C-RNTI and old TMSI. Attacker monitors downlink messages and drops config_update_command which assigns a new TMSI to UE. IF (in standard CAN) no acknowledgement is queried, the UE keeps the old TMSI. New paging messages then get sent to the new TMSI and only after some time to the old one. The attacker could now also drop these and after a certain number of trys, AMF stops with both paging and configuration attempts on new TMSI which means the old TMSI is kept.
    * long term same TMSI is used
  * Cutting off the Device
Adversary knows C-RNTI or TMSI and sends reg_request or ue_dereg_request messages to AMF impersonating victim. AMF discards these requests as they dont contain integrity protection. AMF implicitly drops connection to victim by deregistring it and connecting to impersonator.
    * DoS
* RRC Layer:
  * Denial-of-Service with rrc_setup_request
Attacker knows TMSI and sets up fake UE impersonating victim. Attacker sends rrc_setup_request. Base station implicitly releases connection of the victim's UE and connects to impersonator.
    * Stealthy disconnection of victim UE
  * Installing Null Cipher and Null Integrity
Adversary knows victim's C-RNTI and sets up MitM. gNB sends RRC_security_mode_command, MitM sends back RRC_security_mode_failure and drops RRC_security_mode_complete by the UE. (this failure message has no integrity protection). Now they just both use the prior ciphering and integrity protection which is the "null integrity protection"-agorithm which is only used for control plane messages and for the UE in limited service mode (911). (why is the prior ciphering null???). Now the adversary is a real MitM for control-plane messages. If the fake base station now sends identity_request, the UE answers unencrypted with its SUPI.
    * drops protection of control plane messages, get SUPI
  * Lullaby Attack
Attacker knows C-RNTI and is MitM/fake base station. Base station sends encrypted and integrity protected rrc_reconfiguration message to UE with arbitrary MAC. UE can't verify integrity and moves to "idle" state (releases rrc connection). If UE now has messages to be sent/received, it will again establish the connection and set up the RRC layer security context. Attacker does it again etc., which drains the battery life of the victim.
    * battery drain
  * Incarceration with rrc_reject and rrc_release
Attacker knows C-RNTI or TMSI and sets up two fake base stations. UE connects to fake base station with rcc_setup_request and gets rrc_reject (which is integrity unprotected by default in idle state). Station sets mobility backoff timer which causes UE to remain in idle state for 16 seconds and then tries to reconnect. The station could now do this forever but UE will stop trying after 4 times. Instead fake base station 1 also sends rcc_release with "redirected carrier information" to persuade victim to connecto to fake base station nr 2 and so on.
    * DoS
    * battery drain
* Cross Layer
  * Exposing Device’s TMSI and Paging Occasion
Adversary knows C-RNTI and phone number and is MitM relay and can see the paging broadcast channel of the legitimate base station. The standard does not require acknowledgement of rrc_release messages sent for the UE. Adversary drops this rrc_release message which causes UE not to release the RRC connection and stay in "connected" state. The network assumes the device to be in the "idle" state. Attacker makes phone calls to victim. Network asks base station to broadcast paging messages with the victims TMSI. UE doesn't receive it, as it's in the wrong state. Adversary finds paging messages triggered by his phone calls and infer the seen TMSI as the victim's. Adversay can also learn victim's "paging occasion" by knowing TMSI. (paging occasion = exact time periods when the device polls for services). 
    * With TMSI or paging occasion, adversary can track location or hijack the paging channel to broadcast fake emergency alerts or use it as pre-step for other attacks.
  * Exposing Device's I-RNTI
Same as above, but he learns I-RNTI

Old (detected by prior work or inherited from 4G LTE):
* Downgrade using reject messages (LTEInspector/other paper)(knows C-RNTI or TMSI. UE sends TAU request to fake base station, which answers with TAU reject with reason LTE/5G Services not allowed. not integrity protected)
  * either downgrade to older standard or DoS if not available
* Linkability using authentication_failure (inspired by formal analysis of 5g auth paper) (In this attack, the attacker observes one 5G AKA authentication session and later replays the SN’s message to some subscriber. From the subscriber’s answer (MAC failure or Synchronization failure), the attacker can distinguish between the subscriber observed earlier (in case of Synchronization failure) and a different subscriber (in case of MAC failure). )
  * track subscribers over time
* Paging channel hijacking (LTEInspector "P-1") (Needs S-TMSI or C-RNTI + malicious base station. Fake Base station broadcasts empty paging messages with higher signal power.)
  * Stealthy DoS
* Panic attack (LTEInspector "P-3") (Needs malicious base station. Fake emergency paging messages -> broadcasting for all possible paging occasions of the legitimate base station)
  * Artificial Chaos
* Linkability/Tracking using sec_mode_command (LTEInspector "P-5") (knows C-RNTI has mitm relay. issue paging message to old C-RNTI (PMSI 4G?) when UE answers with new C-RNTI (PMSI?) -> PMSIs linked)
  * location leakage

Formal Analysis of 5G Authentication:
* Authentication Attack 1 (5.2.1): SN thinks it is talking to another UE, but is actually talking to the same.
  * billing on other SUPIs account 
* Authentication Attack 2 (5.2.2): Attacker can impersonate SN to UE without key-confirmation
  * Maybe attacks possible. see paper
* Privacy / Tracing (5.2.3): passive is now better than old 4g 3g etc, but active not. replaying 5g aka auth session SN message to subscriber. from mac failure or synch failure attacker can distinguish between subscriber observed earlier or different subscriber
  * can be used to track subscriber over time

Honorable Mentions:
* DDoS on a new scale
* SDN (slicing etc is all virtual)
* Reliance on few suppliers
* Web technologies
_____________________________________________
Altaf Shaik:
- MNMap
- Bidding down
- Battery Drain
--> These got fixed by the 3GPP in Release 14
