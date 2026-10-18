# Final Report

## Summary

A network and malware investigation was performed on a PCAP capture from 
internal host `10.9.4.103`. The analysis identified malicious file 
delivery, decoded the infection mechanism, and confirmed execution 
behavior through isolated dynamic analysis.

## Findings

### Malicious File Delivery

- `Doc3134.doc` was delivered from `devbyjr.com`. Its macro auto-executes 
  and uses an obfuscated PowerShell command to download and run the 
  next-stage payload.
- `rFrUanXYL.exe` was delivered from `boloshortolandia.com`, matching the 
  URL decoded from the macro.
- Both files were identified as malicious by VirusTotal.

### Execution Behavior

- Dynamic analysis confirmed `rFrUanXYL.exe` executes, copies itself as 
  `paramvolume.exe`, and deletes the original file.
- No persistence mechanism or network activity was observed in the 
  isolated analysis environment.
- VirusTotal sandbox data confirms the file is capable of C2 
  communication, matching `201.146.211.106:7080` observed in the original 
  capture.

### External Communications

Communications with identified suspicious infrastructure were observed:
- `192.241.188.55`
- `93.189.41.44`
- `201.146.211.106`

## Impact

**Confirmed:**
- Malicious files were transferred to the internal host.
- The macro executes automatically and triggers the infection chain.
- `rFrUanXYL.exe` executes and exhibits anti-forensic behavior 
  (self-deletion).
- C2 capability confirmed through external sandbox data.

**Not confirmed:**
- C2 communication in the analysis environment.
- Persistence mechanisms.
- Data theft.
- Lateral movement.

## Recommendations

- Isolate and investigate the affected host.
- Block identified domains, IP addresses, and file hashes.
- Review logs for additional affected systems.
- Monitor for `paramvolume.exe` and related file names as an indicator.
