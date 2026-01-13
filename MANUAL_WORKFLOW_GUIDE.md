# Manual Assessment Workflow Guide
## Using Local LLM for Cisco IOS Security & OSPF Analysis

**Version:** 1.0
**Last Updated:** 2026-01-12
**Purpose:** Step-by-step guide for manual network assessments using locally-hosted LLM

---

## Table of Contents

1. [Overview](#overview)
2. [Prerequisites](#prerequisites)
3. [Security Assessment Workflow](#security-assessment-workflow)
4. [OSPF Assessment Workflow](#ospf-assessment-workflow)
5. [LLM Prompt Templates](#llm-prompt-templates)
6. [Expected Outputs](#expected-outputs)
7. [Troubleshooting](#troubleshooting)
8. [Best Practices](#best-practices)

---

## Overview

This guide provides a manual alternative to automated assessment scripts, enabling you to:
- Collect device configurations manually
- Use a local/company-hosted LLM for analysis
- Generate comprehensive security and OSPF reports
- Maintain security by keeping data within your infrastructure

### When to Use This Workflow

✅ **Use when:**
- Automated scripts cannot be deployed
- Company policy requires manual validation
- You want to use internal LLM infrastructure
- Air-gapped or restricted network environments
- Training or educational purposes

❌ **Don't use when:**
- Automated scripts are available and permitted
- Large fleet of devices (>10 devices)
- Frequent assessments required (daily/weekly)

---

## Prerequisites

### Required Access
- [ ] SSH/Console access to Cisco IOS devices
- [ ] Privilege level 15 or enable access
- [ ] Access to local LLM interface (ChatGPT-like interface, Ollama, LM Studio, etc.)

### Required Tools
- [ ] Terminal emulator (PuTTY, SecureCRT, iTerm2, etc.)
- [ ] Text editor (Notepad++, VS Code, Sublime Text)
- [ ] Web browser (for LLM access)

### Required Knowledge
- Basic Cisco IOS CLI navigation
- Copy/paste operations
- Basic markdown understanding (optional)

### Device Requirements
- Cisco IOS or IOS-XE devices
- SSH or console connectivity
- Enable password/secret

---

## Security Assessment Workflow

### Overview
This workflow evaluates 15+ security controls against NSA, NIST, and CIS benchmarks.

**Time Estimate:** 15-20 minutes per device
**Output:** Comprehensive security report with findings and remediation

---

### Step 1: Connect to Device

**1.1** Open your terminal emulator

**1.2** SSH to the target device:
```bash
ssh admin@192.168.1.130
```

**1.3** Enter enable mode:
```
Router> enable
Password: [enter enable password]
Router#
```

**1.4** Verify you're in privileged mode (should see `#` prompt)

---

### Step 2: Collect Configuration Data

**2.1** Open a new text file on your computer (e.g., `cat8000v1_config.txt`)

**2.2** Run the following command and copy the ENTIRE output:

```cisco
show running-config
```

**Important Notes:**
- Let the command complete fully (may take 30-60 seconds)
- Capture ALL output (scroll up if needed)
- Don't edit or modify the output
- Include the hostname line at the top

**2.3** Paste the complete output into your text file

**2.4** Save the file with device name and date:
```
cat8000v1_running-config_2026-01-12.txt
```

---

### Step 3: Collect Additional Show Commands

For more comprehensive analysis, also collect these outputs:

**3.1** Show version (for device info):
```cisco
show version
```
Copy output to a new section in your text file, labeled "SHOW VERSION"

**3.2** Show IP interface brief (for interface status):
```cisco
show ip interface brief
```
Copy output, label as "SHOW IP INTERFACE BRIEF"

**3.3** Show users (for active sessions):
```cisco
show users
```
Copy output, label as "SHOW USERS"

**Example text file structure:**
```
========================================
DEVICE: cat8000v1
DATE: 2026-01-12
IP: 192.168.1.130
========================================

=== SHOW RUNNING-CONFIG ===
[paste complete running-config here]

=== SHOW VERSION ===
[paste show version output here]

=== SHOW IP INTERFACE BRIEF ===
[paste interface output here]

=== SHOW USERS ===
[paste users output here]
```

---

### Step 4: Prepare LLM Prompt

**4.1** Open your local LLM interface in a web browser

**4.2** Copy the Security Assessment Prompt Template (see Section 5.1)

**4.3** Replace `[PASTE DEVICE OUTPUT HERE]` with your collected output

**4.4** Ensure the prompt includes:
- Device name/hostname
- The complete running-config
- All show command outputs

---

### Step 5: Submit to LLM

**5.1** Paste the complete prompt into the LLM chat interface

**5.2** Submit the prompt

**5.3** Wait for analysis (may take 1-3 minutes depending on LLM)

**5.4** Review the generated report

---

### Step 6: Save and Review Report

**6.1** Copy the LLM's complete response

**6.2** Save to a markdown file:
```
ios_security_assessment_cat8000v1_2026-01-12.md
```

**6.3** Review the findings:
- Critical issues (red flags)
- High priority items
- Medium/Low priority items
- Remediation commands

**6.4** Verify the report includes:
- [ ] Executive summary
- [ ] Security score/percentage
- [ ] Detailed findings table
- [ ] Remediation commands
- [ ] Compliance mapping

---

### Step 7: Document and Track

**7.1** Store the report in your documentation system

**7.2** Create action items for each finding

**7.3** Schedule remediation based on severity

**7.4** Plan follow-up assessment after fixes

---

## OSPF Assessment Workflow

### Overview
This workflow validates OSPF configuration, neighbor adjacencies, and identifies common routing issues.

**Time Estimate:** 20-25 minutes per device
**Output:** Comprehensive OSPF health report with troubleshooting guidance

---

### Step 1: Connect to Device

(Same as Security Assessment Step 1)

---

### Step 2: Collect OSPF Show Commands

**2.1** Create a new text file: `cat8000v1_ospf_data_2026-01-12.txt`

**2.2** Collect the following commands in sequence:

#### Command 1: OSPF Process Status
```cisco
show ip ospf
```
**What it shows:** OSPF process ID, Router ID, areas, timers
**Copy:** Complete output to your text file, label "SHOW IP OSPF"

#### Command 2: OSPF Neighbors
```cisco
show ip ospf neighbor
```
**What it shows:** Neighbor states, priorities, dead time
**Copy:** Complete output, label "SHOW IP OSPF NEIGHBOR"

#### Command 3: OSPF Neighbor Details
```cisco
show ip ospf neighbor detail
```
**What it shows:** Detailed neighbor info, DR/BDR, capabilities
**Copy:** Complete output, label "SHOW IP OSPF NEIGHBOR DETAIL"

#### Command 4: OSPF Interfaces Brief
```cisco
show ip ospf interface brief
```
**What it shows:** All OSPF-enabled interfaces, areas, costs, neighbor counts
**Copy:** Complete output, label "SHOW IP OSPF INTERFACE BRIEF"

#### Command 5: OSPF Interfaces Detailed
```cisco
show ip ospf interface
```
**What it shows:** Timers, network types, authentication, DR/BDR info
**Copy:** Complete output, label "SHOW IP OSPF INTERFACE"

#### Command 6: OSPF Database
```cisco
show ip ospf database
```
**What it shows:** LSA database summary, all LSA types
**Copy:** Complete output, label "SHOW IP OSPF DATABASE"

#### Command 7: OSPF Router Database
```cisco
show ip ospf database router
```
**What it shows:** Detailed router LSAs, link states
**Copy:** Complete output, label "SHOW IP OSPF DATABASE ROUTER"

#### Command 8: IP Protocols
```cisco
show ip protocols
```
**What it shows:** Routing protocols enabled, networks advertised
**Copy:** Complete output, label "SHOW IP PROTOCOLS"

#### Command 9: OSPF Routes
```cisco
show ip route ospf
```
**What it shows:** OSPF routes in routing table
**Copy:** Complete output, label "SHOW IP ROUTE OSPF"

#### Command 10: OSPF Configuration
```cisco
show running-config | section router ospf
```
**What it shows:** OSPF router configuration
**Copy:** Complete output, label "OSPF CONFIGURATION"

#### Command 11: Interface Configuration
```cisco
show running-config | section interface
```
**What it shows:** All interface configurations
**Copy:** Complete output, label "INTERFACE CONFIGURATION"

---

### Step 3: Organize OSPF Data

**3.1** Your text file should now look like this:

```
========================================
DEVICE: cat8000v1
DATE: 2026-01-12
IP: 192.168.1.130
ASSESSMENT TYPE: OSPF Routing
========================================

=== SHOW IP OSPF ===
[output]

=== SHOW IP OSPF NEIGHBOR ===
[output]

=== SHOW IP OSPF NEIGHBOR DETAIL ===
[output]

=== SHOW IP OSPF INTERFACE BRIEF ===
[output]

=== SHOW IP OSPF INTERFACE ===
[output]

=== SHOW IP OSPF DATABASE ===
[output]

=== SHOW IP OSPF DATABASE ROUTER ===
[output]

=== SHOW IP PROTOCOLS ===
[output]

=== SHOW IP ROUTE OSPF ===
[output]

=== OSPF CONFIGURATION ===
[output]

=== INTERFACE CONFIGURATION ===
[output]
```

**3.2** Save the file

---

### Step 4: Prepare OSPF LLM Prompt

**4.1** Open your local LLM interface

**4.2** Copy the OSPF Assessment Prompt Template (see Section 5.2)

**4.3** Replace `[PASTE OSPF DATA HERE]` with your complete data file

**4.4** Verify all 11 command outputs are included

---

### Step 5: Submit to LLM

**5.1** Paste the complete OSPF prompt into LLM

**5.2** Submit and wait for analysis (2-4 minutes)

**5.3** LLM will analyze:
- OSPF process health
- Neighbor adjacency states
- Timer configurations
- Network type settings
- Area configuration
- Authentication status
- Route propagation
- Database consistency

---

### Step 6: Review OSPF Report

**6.1** LLM will generate a report including:
- OSPF Health Score (0-100%)
- Neighbor adjacency analysis
- Common issues detected
- Root cause analysis
- Remediation commands
- Troubleshooting workflow

**6.2** Save the report:
```
ospf_routing_assessment_cat8000v1_2026-01-12.md
```

**6.3** Review critical findings:
- [ ] Are all neighbors in FULL state?
- [ ] Any stuck neighbors (INIT, EXSTART, etc.)?
- [ ] Timer mismatches?
- [ ] Authentication issues?
- [ ] MTU mismatches?
- [ ] Area configuration problems?

---

### Step 7: Multi-Device OSPF Assessment

For OSPF issues between routers, collect data from BOTH sides:

**7.1** Repeat Steps 1-3 for the second router

**7.2** Create a combined data file:

```
========================================
MULTI-DEVICE OSPF ASSESSMENT
DATE: 2026-01-12
========================================

########################################
# DEVICE 1: cat8000v1 (192.168.1.130)
########################################

=== SHOW IP OSPF ===
[device 1 output]

[... all commands for device 1 ...]

########################################
# DEVICE 2: cat8000v2 (192.168.1.131)
########################################

=== SHOW IP OSPF ===
[device 2 output]

[... all commands for device 2 ...]
```

**7.3** Use the Multi-Device OSPF Prompt Template (Section 5.3)

**7.4** LLM will perform comparative analysis:
- Identify configuration mismatches
- Compare neighbor states from both sides
- Detect timer inconsistencies
- Find authentication mismatches
- Analyze why adjacency isn't forming

---

## LLM Prompt Templates

### 5.1 Security Assessment Prompt Template

Copy and paste this template into your LLM:

```
You are a Cisco IOS security expert conducting a comprehensive security hardening assessment based on industry best practices including NSA Router Security Configuration Guide, NIST SP 800-53, and CIS Cisco IOS Benchmark.

TASK:
Analyze the provided Cisco IOS device configuration and generate a detailed security assessment report.

DEVICE INFORMATION:
- Device Name: cat8000v1
- IP Address: 192.168.1.130
- Date: 2026-01-12

DEVICE OUTPUT:
[PASTE DEVICE OUTPUT HERE - Include show running-config and all show commands]

ASSESSMENT REQUIREMENTS:

Evaluate the following 15 security controls:

1. AAA Configuration
   - Check if "aaa new-model" is enabled
   - Verify authentication and authorization are configured
   - Severity: Critical

2. Enable Secret
   - Check for "enable secret" (not "enable password")
   - Verify it's using strong encryption
   - Severity: Critical

3. Service Password Encryption
   - Check for "service password-encryption"
   - Severity: High

4. SSH Configuration
   - Check for "ip ssh version 2"
   - Verify crypto keys are generated
   - Ensure SSH timeout and retries are set
   - Severity: High

5. VTY Line Security
   - Check for exec-timeout on VTY lines
   - Verify "transport input ssh" (not telnet)
   - Check for access-class ACL
   - Severity: High

6. Console Security
   - Check console line has exec-timeout
   - Verify login authentication is configured
   - Severity: Medium

7. SNMP Security
   - Check if SNMPv3 is used (not v1/v2c)
   - Verify no default community strings
   - Severity: High (if SNMP is used)

8. HTTP/HTTPS Server
   - Check if "no ip http server" or "ip http secure-server" only
   - Severity: High

9. Logging Configuration
   - Check for "logging buffered"
   - Verify logging host (syslog) is configured
   - Check for "service timestamps"
   - Severity: Medium

10. NTP Configuration
    - Check for "ntp server"
    - Verify NTP authentication if configured
    - Severity: Medium

11. Login Banners
    - Check for "banner login" or "banner motd"
    - Severity: Low

12. Unnecessary Services
    - Check these are NOT present:
      - ip finger
      - ip bootp server
      - service tcp-small-servers
      - service udp-small-servers
      - ip identd
      - service pad
    - Severity: Medium

13. IP Source Routing
    - Check for "no ip source-route"
    - Severity: Medium

14. CDP Configuration
    - Check if CDP is disabled globally or on untrusted interfaces
    - Severity: Low

15. Auxiliary Port
    - Check line aux 0 has "no exec" or "transport input none"
    - Severity: Medium

REPORT FORMAT:

Generate a markdown report with the following structure:

# Cisco IOS Security Assessment Report

**Generated:** [Current Date]
**Device:** cat8000v1
**IP Address:** 192.168.1.130

## Executive Summary
[Brief overview of security posture]

## Security Score
[Calculate score: (Passed Checks / Total Checks) × 100]
Overall Score: XX%

## Summary Statistics
- ✅ Passed: X checks
- ❌ Failed: X checks
- ⚠️ Partial: X checks
- Total Checks: 15

## Detailed Findings

Create a table with these columns:
| Security Control | Status | Severity | Current State | Recommendation |

For each of the 15 controls:
- Status: ✅ Pass, ❌ Fail, or ⚠️ Partial
- Severity: Critical, High, Medium, or Low
- Current State: What configuration was found (or not found)
- Recommendation: What should be done

## Critical Issues
[List all Critical and High severity failures with detailed explanation]

## Remediation Commands

For each failed check, provide exact IOS commands to fix:

**[Control Name]** (Severity)
```
[exact IOS commands]
```

## Compliance Mapping
Map findings to:
- NSA Router Security Configuration Guide
- NIST SP 800-53 controls (IA-2, AC-2, SC-8, etc.)
- CIS Cisco IOS Benchmark sections

## Recommendations
1. Priority 1 (Critical): [List critical items]
2. Priority 2 (High): [List high items]
3. Priority 3 (Medium): [List medium items]
4. Priority 4 (Low): [List low items]

## Next Steps
[Action plan for remediation]

---

IMPORTANT:
- Be specific about what you found (or didn't find) in the configuration
- Provide exact line references when possible
- Include actual configuration snippets in "Current State"
- Make remediation commands copy-paste ready
- Use emojis for visual clarity (✅ ❌ ⚠️)
- Be thorough but concise

BEGIN ANALYSIS NOW.
```

---

### 5.2 OSPF Assessment Prompt Template (Single Device)

Copy and paste this template into your LLM:

```
You are a Cisco OSPF routing expert conducting comprehensive OSPF validation and troubleshooting analysis based on Cisco best practices and common production network issues.

TASK:
Analyze the provided OSPF data from a Cisco IOS device and generate a detailed OSPF health and troubleshooting report.

DEVICE INFORMATION:
- Device Name: cat8000v1
- IP Address: 192.168.1.130
- Date: 2026-01-12

OSPF DATA COLLECTED:
[PASTE COMPLETE OSPF DATA HERE - All 11 show commands]

ASSESSMENT REQUIREMENTS:

Analyze these 9 OSPF health checks:

1. OSPF Process Status
   - Is OSPF process running?
   - What is the Router ID?
   - What process ID is used?
   - Are networks being advertised?
   - Severity: Critical if not running

2. OSPF Neighbor Adjacencies
   - How many neighbors exist?
   - What states are they in? (FULL is healthy)
   - Any stuck neighbors? (INIT, EXSTART, 2WAY, EXCHANGE, LOADING)
   - If stuck, identify likely cause:
     * INIT = one-way communication
     * EXSTART = MTU mismatch likely
     * 2WAY = normal on broadcast, issue on P2P
     * EXCHANGE/LOADING = database sync issues
   - Severity: Critical if no neighbors or stuck states

3. OSPF Timer Configuration
   - What are hello/dead intervals?
   - Are timers consistent across interfaces?
   - Is dead interval 4x hello? (standard ratio)
   - Severity: High if mismatch (prevents adjacency)

4. OSPF Network Types
   - What network types are configured? (BROADCAST, POINT_TO_POINT, etc.)
   - Are network types appropriate for link types?
   - Any mismatches between interfaces?
   - Severity: Medium (mismatches prevent adjacency)

5. OSPF Area Configuration
   - What areas are configured?
   - Is Area 0 (backbone) present?
   - For multi-area, is backbone properly configured?
   - Any stub or NSSA areas?
   - Severity: High if multi-area without backbone

6. OSPF Authentication
   - Is authentication configured?
   - Type: None, Simple, or MD5?
   - Is authentication consistent across interfaces?
   - Severity: Medium (security), High if mismatch (breaks adjacency)

7. OSPF Routes in Routing Table
   - How many OSPF routes exist?
   - Types: Intra-area (O), Inter-area (O IA), External (O E1/E2)
   - Are routes being learned as expected?
   - Severity: High if no routes despite neighbors

8. OSPF Passive Interfaces
   - Is passive-interface default configured?
   - Which interfaces are explicitly non-passive?
   - Security best practice check
   - Severity: Low (security concern)

9. OSPF Database Integrity
   - Is the OSPF database populated?
   - What LSA types exist? (Router, Network, Summary, External)
   - Any "not-reachable" errors?
   - Database age and sequence numbers
   - Severity: Critical if empty, High if not-reachable errors

REPORT FORMAT:

Generate a markdown report with this structure:

# OSPF Routing Assessment Report

**Generated:** [Current Date]
**Device:** cat8000v1
**IP Address:** 192.168.1.130

## Executive Summary
[Brief OSPF health overview]

## OSPF Health Score
Calculate: (Passed Checks / Total Checks) × 100
- 80-100%: 🟢 Healthy
- 50-79%: 🟡 Needs Attention
- 0-49%: 🔴 Critical

**OSPF Health Score:** XX% [Status Emoji]

## Summary Statistics
- ✅ Passed: X checks
- ❌ Failed: X checks
- ⚠️ Partial: X checks
- Total Checks: 9

## Detailed Findings

| OSPF Check | Status | Severity | Current State | Recommendation |

For each of the 9 checks:
- Status: ✅ Pass, ❌ Fail, ⚠️ Partial
- Severity: Critical, High, Medium, Low, Info
- Current State: Detailed findings with evidence from output
- Recommendation: What to do next

## Critical Issues & Root Cause Analysis

For any failed or stuck states, provide:
1. **Issue:** [What's wrong]
2. **Evidence:** [Quote from show commands]
3. **Root Cause:** [Why it's happening]
4. **Impact:** [Effect on network]
5. **Fix:** [Exact remediation steps]

## Neighbor Adjacency Analysis

If neighbors exist:
- List each neighbor with state, interface, dead time
- For FULL neighbors: ✅ Healthy
- For stuck neighbors: Detailed troubleshooting

If no neighbors:
- Possible causes:
  * OSPF not enabled on interfaces
  * Passive interface configured
  * Authentication mismatch
  * Area mismatch
  * Network type issues
  * No Layer 2 connectivity
  * ACL blocking multicast (224.0.0.5)

## Configuration Summary

- **Router ID:** X.X.X.X
- **Process ID:** XX
- **Areas:** [List]
- **OSPF Interfaces:** [Count] active
- **Neighbors:** [Count] (X FULL, X other states)
- **OSPF Routes:** [Count] (breakdown by type)

## Remediation Commands

For each issue found:

**[Issue Name]** (Severity)
```
[Exact IOS commands to fix]
```

## Common OSPF Issues Reference

Include quick reference for:
- Stuck in INIT → causes and fixes
- Stuck in EXSTART → MTU troubleshooting
- Stuck in 2WAY → network type check
- No neighbors → comprehensive checklist
- No routes → verification steps

## Troubleshooting Workflow

Provide step-by-step commands for investigating issues:

1. Verify OSPF is running
   ```
   show ip ospf
   show ip protocols
   ```

2. Check interfaces
   ```
   show ip ospf interface brief
   show ip ospf interface [interface]
   ```

3. Verify neighbors
   ```
   show ip ospf neighbor
   show ip ospf neighbor detail
   ```

[Continue with full workflow]

## Recommendations

Priority-ordered action items:
1. [Critical items requiring immediate attention]
2. [High priority items]
3. [Medium priority items]
4. [Best practice improvements]

---

IMPORTANT ANALYSIS GUIDELINES:

- Quote actual output in your findings (show evidence)
- For stuck neighbors, identify the specific cause
- Compare timers across interfaces for consistency
- Check for "Adv Router is not-reachable" in database (common issue)
- Look for 0 neighbors on interfaces where neighbors are expected
- Analyze network types (BROADCAST vs POINT_TO_POINT)
- Check authentication settings (or lack thereof)
- Verify areas are correctly configured
- Use emojis for clarity: ✅ ❌ ⚠️ 🟢 🟡 🔴
- Be specific about interface names and IP addresses
- Provide actionable, copy-paste ready remediation commands

BEGIN ANALYSIS NOW.
```

---

### 5.3 OSPF Multi-Device Assessment Prompt Template

Copy and paste this template for troubleshooting between two routers:

```
You are a Cisco OSPF routing expert troubleshooting OSPF adjacency issues between two routers.

TASK:
Analyze OSPF data from TWO routers and identify why OSPF adjacency is or isn't forming, providing comparative analysis and root cause determination.

NETWORK TOPOLOGY:
Device 1: cat8000v1 (192.168.1.130)
Device 2: cat8000v2 (192.168.1.131)

OSPF DATA:
[PASTE COMPLETE DATA FROM BOTH DEVICES]

COMPARATIVE ANALYSIS REQUIREMENTS:

1. **OSPF Process Comparison**
   - Do both routers have OSPF running?
   - Are process IDs the same? (not required, but note it)
   - What are the Router IDs on each side?

2. **Network Connectivity**
   - Are the routers on the same subnet?
   - Do interfaces show "up/up"?
   - Can they reach each other (check ARP)?

3. **Neighbor State Analysis**
   - What does Router A see for Router B's state?
   - What does Router B see for Router A's state?
   - If states differ, why?

4. **Configuration Mismatch Detection**
   Compare these between devices:
   - **Hello/Dead Timers** - Must match exactly
   - **Area Numbers** - Must match for adjacency
   - **Authentication** - Type and key must match
   - **Network Types** - Should match (BROADCAST, P2P, etc.)
   - **MTU** - Check for mismatches (causes EXSTART stuck)
   - **Subnet Masks** - Must be same subnet

5. **Interface-by-Interface Analysis**
   For each potential OSPF link:
   - Interface names and IPs on both sides
   - OSPF enabled? (show in ospf interface brief)
   - Network type on each side
   - Timers on each side
   - Authentication on each side
   - Neighbor count on each side
   - Any issues detected?

6. **Common Issue Detection**
   Check for:
   - Authentication on one side but not the other
   - Different authentication types/keys
   - Timer mismatch (hello or dead intervals differ)
   - Area mismatch (interface in different area)
   - Passive interface on one or both sides
   - MTU mismatch (seen in neighbor detail)
   - Network type mismatch
   - One router sees neighbor, other doesn't (one-way)

REPORT FORMAT:

# OSPF Multi-Device Troubleshooting Report

**Generated:** [Date]
**Devices:** cat8000v1 ↔ cat8000v2

## Executive Summary
[High-level status of OSPF relationship]

## Topology Diagram

```
cat8000v1 (1.1.1.1)              cat8000v2 (2.2.2.2)
IP: 192.168.1.130                IP: 192.168.1.131

Gi1: 192.168.1.130/24 --- [Network] --- 192.168.1.131/24 :Gi1
     [Adjacency Status]

Gi2: 10.10.10.1/24    --- [Network] --- 10.10.10.2/24 :Gi2
     [Adjacency Status]
```

## Adjacency Status Matrix

| Interface Pair | cat8000v1 Status | cat8000v2 Status | Result |
|----------------|------------------|------------------|--------|
| Gi1 ↔ Gi1 | [State] | [State] | ✅/❌ |
| Gi2 ↔ Gi2 | [State] | [State] | ✅/❌ |

## Detailed Per-Link Analysis

### Link 1: GigabitEthernet1 (192.168.1.0/24)

**cat8000v1 Configuration:**
- IP Address: X.X.X.X/XX
- OSPF Area: X
- Network Type: XXXXX
- Hello/Dead: XX/XX
- Authentication: XXXXX
- Neighbor State: XXXXX
- Neighbor Count: X

**cat8000v2 Configuration:**
- IP Address: X.X.X.X/XX
- OSPF Area: X
- Network Type: XXXXX
- Hello/Dead: XX/XX
- Authentication: XXXXX
- Neighbor State: XXXXX
- Neighbor Count: X

**Comparison:**
✅ Match: [List matching parameters]
❌ Mismatch: [List mismatches with details]

**Analysis:**
[Detailed explanation of why this link is working or not]

**Issue Detected:** [If any]
**Root Cause:** [Explain the specific reason]
**Fix:** [Exact commands for both routers]

[Repeat for each link]

## Root Cause Summary

### Primary Issues Found:

1. **[Issue #1 Name]**
   - **Affected Link:** [Interface pair]
   - **Problem:** [What's wrong]
   - **Evidence:** [Quotes from show commands]
   - **Why It's Preventing Adjacency:** [Explanation]
   - **Severity:** Critical/High/Medium

[Repeat for each issue]

## Configuration Comparison Table

| Parameter | cat8000v1 Gi2 | cat8000v2 Gi2 | Match? |
|-----------|---------------|---------------|--------|
| OSPF Area | 0 | 0 | ✅ |
| Network Type | BROADCAST | BROADCAST | ✅ |
| Hello Timer | 10 | 10 | ✅ |
| Dead Timer | 40 | 40 | ✅ |
| Authentication | None | Simple | ❌ |
| Auth Key | - | cisco123 | ❌ |
| MTU | 1500 | 1500 | ✅ |

## Remediation Plan

### Fix #1: [Issue Name]

**On cat8000v1:**
```
configure terminal
interface GigabitEthernet2
 [commands]
end
```

**On cat8000v2:**
```
configure terminal
interface GigabitEthernet2
 [commands]
end
```

**Expected Result:** [What should happen after applying fix]

[Repeat for each fix]

## Verification Steps

After applying fixes, run these commands on BOTH routers:

1. **Verify neighbor adjacency:**
   ```
   show ip ospf neighbor
   ```
   Expected: Should see neighbor in FULL state

2. **Check interface status:**
   ```
   show ip ospf interface brief
   ```
   Expected: Nbrs F/C should show 1/1

3. **Verify routes:**
   ```
   show ip route ospf
   ```
   Expected: Should now see OSPF routes from neighbor

4. **Check database synchronization:**
   ```
   show ip ospf database
   ```
   Expected: Should see LSAs from both routers

## Summary

**Total Issues Found:** X
**Critical:** X
**High:** X
**Medium:** X

**Action Required:**
1. [Priority 1 action]
2. [Priority 2 action]
3. [Priority 3 action]

**Estimated Time to Fix:** XX minutes

---

ANALYSIS GUIDELINES:

- Always compare configurations side-by-side
- For each parameter, explicitly state if it matches or differs
- Provide evidence from actual command output
- Identify THE SPECIFIC REASON adjacency isn't forming (not just "maybe")
- Give exact commands for BOTH routers when needed
- Use visual indicators: ✅ for match, ❌ for mismatch
- Be thorough in checking authentication (type and key)
- Don't assume - verify every potential mismatch point
- Prioritize issues by impact (authentication mismatch is critical)

BEGIN COMPARATIVE ANALYSIS NOW.
```

---

## Expected Outputs

### Security Assessment Expected Output

You should receive a comprehensive report including:

**Structure:**
1. ✅ Executive Summary (2-3 paragraphs)
2. ✅ Security Score (percentage and rating)
3. ✅ Summary Statistics (passed/failed/partial counts)
4. ✅ Detailed Findings Table (15 rows, one per control)
5. ✅ Critical Issues Section (expanded detail on failures)
6. ✅ Remediation Commands (organized by severity)
7. ✅ Compliance Mapping (NSA, NIST, CIS references)
8. ✅ Recommendations (prioritized action plan)

**Quality Indicators:**
- Report should be 10-15 pages in markdown
- Each finding includes evidence from configuration
- Remediation commands are copy-paste ready
- Severity ratings are accurate (Critical/High/Medium/Low)
- Compliance references are specific (e.g., "NIST IA-2, AC-2")

**Red Flags** (indicates poor analysis):
- ❌ Generic findings without device-specific evidence
- ❌ Missing remediation commands
- ❌ No severity ratings
- ❌ Short report (<5 pages)
- ❌ No compliance mapping

---

### OSPF Assessment Expected Output

You should receive a detailed OSPF health report including:

**Structure:**
1. ✅ Executive Summary with health score
2. ✅ OSPF Health Score (0-100% with color-coded status)
3. ✅ Summary Statistics (9 checks with pass/fail)
4. ✅ Detailed Findings Table (9 rows)
5. ✅ Critical Issues & Root Cause Analysis
6. ✅ Neighbor Adjacency Analysis (per-neighbor breakdown)
7. ✅ Configuration Summary (Router ID, areas, interfaces)
8. ✅ Remediation Commands (for each issue)
9. ✅ Common OSPF Issues Reference
10. ✅ Troubleshooting Workflow

**For Multi-Device OSPF:**
- ✅ Topology diagram
- ✅ Adjacency status matrix
- ✅ Per-link detailed analysis
- ✅ Configuration comparison table
- ✅ Side-by-side parameter comparison
- ✅ Root cause identification (specific, not generic)
- ✅ Fixes for BOTH routers

**Quality Indicators:**
- Specific evidence quoted from show commands
- Clear explanation of WHY adjacency is/isn't forming
- Identification of exact mismatch (e.g., "cat8000v2 has auth, cat8000v1 does not")
- Commands provided for both sides when needed
- Health score accurately reflects OSPF state

**Red Flags:**
- ❌ Can't identify specific cause of stuck neighbor
- ❌ Generic "may be a problem" statements
- ❌ Missing comparison between devices
- ❌ No evidence from actual output
- ❌ Doesn't quote interface names, IPs, or states

---

## Troubleshooting

### Common Issues and Solutions

#### Issue 1: LLM Returns Incomplete Report

**Symptoms:**
- Report cuts off mid-section
- Missing remediation commands
- No compliance mapping

**Solutions:**
1. Break prompt into smaller parts:
   - First prompt: Request analysis only
   - Second prompt: "Now provide remediation commands"
   - Third prompt: "Now provide compliance mapping"

2. Use "Continue" or "Please complete the report" prompt

3. Check LLM token limits - may need shorter input

#### Issue 2: LLM Gives Generic Analysis

**Symptoms:**
- No device-specific evidence
- Doesn't quote from configuration
- Generic recommendations

**Solutions:**
1. Emphasize in prompt: "Quote exact configuration lines"
2. Add: "Provide evidence from the actual output"
3. Ask follow-up: "Show me exactly where in the config you found this"

#### Issue 3: Can't Determine OSPF Root Cause

**Symptoms:**
- "Could be authentication or timers or area mismatch"
- Multiple possible causes listed
- No definitive answer

**Solutions:**
1. Provide more specific question:
   ```
   Compare authentication configuration between cat8000v1 and cat8000v2
   on interface GigabitEthernet2. Show me the exact configuration lines
   from each device. Do they match?
   ```

2. Use targeted analysis:
   ```
   Focus ONLY on GigabitEthernet2. List every OSPF parameter on both
   sides in a comparison table. Identify ANY mismatches.
   ```

#### Issue 4: Configuration Copy/Paste Issues

**Symptoms:**
- LLM complains about invalid format
- Can't parse configuration
- Analysis fails

**Solutions:**
1. Ensure clean copy (no terminal escape codes)
2. Remove "Building configuration..." headers
3. Remove "end" at end of running-config
4. Ensure no lines are word-wrapped

#### Issue 5: LLM Hallucinates Commands

**Symptoms:**
- Remediation commands that don't exist in IOS
- Incorrect syntax
- Wrong command parameters

**Solutions:**
1. Add to prompt: "Only provide commands that exist in Cisco IOS"
2. Verify commands against Cisco documentation
3. Test commands on lab device first
4. Ask LLM: "Are you certain this command exists in IOS?"

---

## Best Practices

### Data Collection Best Practices

1. **Be Thorough**
   - Capture ALL output (scroll to end)
   - Don't truncate long outputs
   - Include all requested show commands

2. **Be Organized**
   - Label each section clearly
   - Use consistent formatting
   - Include device name, IP, date

3. **Be Clean**
   - Remove terminal artifacts (^C, escape codes)
   - Don't include command prompt in paste
   - Remove duplicate outputs

4. **Be Accurate**
   - Copy actual output (don't retype)
   - Don't edit or sanitize yet (do that after if needed)
   - Verify you copied the right device's output

### LLM Interaction Best Practices

1. **Start with Context**
   - Always include device name and IP
   - Mention the assessment type clearly
   - State your goal (security review, OSPF troubleshooting)

2. **Be Specific in Questions**
   - Good: "Is authentication configured on Gi2 on both routers and does it match?"
   - Bad: "Why won't OSPF work?"

3. **Ask Follow-Up Questions**
   - "Can you explain that in more detail?"
   - "Show me the exact configuration causing this issue"
   - "What would happen if I applied this fix?"

4. **Verify Critical Findings**
   - Double-check critical issues with device
   - Confirm mismatches LLM identifies
   - Validate remediation commands before applying

### Report Management Best Practices

1. **File Naming Convention**
   ```
   [assessment-type]_[device]_[YYYY-MM-DD].md

   Examples:
   ios_security_assessment_cat8000v1_2026-01-12.md
   ospf_routing_assessment_cat8000v1_2026-01-12.md
   ospf_multi_device_cat8000v1-v2_2026-01-12.md
   ```

2. **Version Control**
   - Keep historical reports
   - Track changes over time
   - Compare before/after remediation

3. **Documentation**
   - Include raw data files
   - Link reports to tickets/issues
   - Document what was fixed

4. **Security**
   - Sanitize sensitive data if needed (passwords, IP addresses)
   - Store reports securely
   - Control access to configuration files

### Remediation Best Practices

1. **Always Test First**
   - Never apply commands to production without testing
   - Use lab environment or change window
   - Have rollback plan ready

2. **Apply Changes Systematically**
   - Address critical issues first
   - One change at a time
   - Verify after each change

3. **Document Changes**
   - Record what was changed
   - When and by whom
   - Before/after configuration

4. **Re-Assess After Changes**
   - Run assessment again
   - Verify issues are resolved
   - Check for new issues introduced

---

## Quick Reference

### Command Cheat Sheet

**Essential Security Assessment Commands:**
```
show running-config
show version
show ip interface brief
show users
show ip ssh
show snmp
show ntp status
show logging
```

**Essential OSPF Assessment Commands:**
```
show ip ospf
show ip ospf neighbor
show ip ospf neighbor detail
show ip ospf interface brief
show ip ospf interface
show ip ospf database
show ip ospf database router
show ip protocols
show ip route ospf
show running-config | section router ospf
show running-config | section interface
```

**Quick Verification Commands:**
```
show ip ospf neighbor | include FULL
show running-config | include aaa
show running-config | include ip ssh
show ip ospf interface brief | exclude down
```

---

## Example: Complete Security Assessment Workflow

Let's walk through a complete example:

### Step-by-Step Example

**Step 1:** SSH to device
```bash
ssh admin@192.168.1.130
Password: Cisco!123
cat8000v1#
```

**Step 2:** Collect running-config
```cisco
cat8000v1# show running-config
Building configuration...

Current configuration : 4532 bytes
!
! Last configuration change at 13:45:12 UTC Sun Jan 12 2026
!
version 17.12
service timestamps debug datetime msec
service timestamps log datetime msec
platform qfp utilization monitor load 80
...
[copy EVERYTHING until you see "end"]
```

**Step 3:** Save to file `cat8000v1_config_2026-01-12.txt`

**Step 4:** Open LLM, paste Security Assessment Prompt Template

**Step 5:** Replace `[PASTE DEVICE OUTPUT HERE]` with your saved config

**Step 6:** Submit to LLM

**Step 7:** Receive report like this:

```markdown
# Cisco IOS Security Assessment Report

**Generated:** 2026-01-12
**Device:** cat8000v1
**IP Address:** 192.168.1.130

## Security Score
Overall Score: 20%

## Summary Statistics
- ✅ Passed: 3 checks
- ❌ Failed: 10 checks
- ⚠️ Partial: 2 checks

## Critical Issues

### AAA Configuration - FAILED
**Current State:** No "aaa new-model" found in configuration
**Evidence:** Searched running-config, no AAA commands present
**Recommendation:** Implement AAA immediately
...
```

**Step 8:** Save report as `ios_security_assessment_cat8000v1_2026-01-12.md`

**Step 9:** Create action items based on findings

**Step 10:** Schedule remediation

---

## Example: Complete OSPF Multi-Device Assessment

### Scenario: OSPF Not Working Between Two Routers

**Step 1:** Collect data from cat8000v1 (all 11 commands)

**Step 2:** Collect data from cat8000v2 (all 11 commands)

**Step 3:** Create combined file:

```
========================================
MULTI-DEVICE OSPF ASSESSMENT
========================================

########################################
# DEVICE 1: cat8000v1
########################################

=== SHOW IP OSPF ===
...

[all commands for device 1]

########################################
# DEVICE 2: cat8000v2
########################################

=== SHOW IP OSPF ===
...

[all commands for device 2]
```

**Step 4:** Use Multi-Device OSPF Prompt Template

**Step 5:** Submit to LLM

**Step 6:** Receive detailed comparison:

```markdown
# OSPF Multi-Device Troubleshooting Report

## Root Cause Found

**Issue:** Authentication Mismatch on GigabitEthernet2

**Evidence:**
- cat8000v1 Gi2: No authentication configured
- cat8000v2 Gi2: Simple password authentication with key "cisco123"

**Why It's Preventing Adjacency:**
OSPF requires authentication to match on both sides. When cat8000v2
sends authenticated hellos, cat8000v1 rejects them because it's not
configured for authentication.

**Fix:**

On cat8000v1:
```
configure terminal
interface GigabitEthernet2
 ip ospf authentication
 ip ospf authentication-key cisco123
end
write memory
```
```

**Step 7:** Apply the fix

**Step 8:** Verify adjacency forms:
```
cat8000v1# show ip ospf neighbor
Neighbor ID     Pri   State           Dead Time   Address         Interface
2.2.2.2           1   FULL/DR         00:00:35    10.10.10.2      GigabitEthernet2
```

✅ **Success!** Neighbor is now in FULL state.

---

## Summary

This manual workflow allows you to leverage local LLM infrastructure for comprehensive network assessments without requiring automated script deployment. The key is:

1. **Thorough data collection** - Get all the right show commands
2. **Well-structured prompts** - Use the templates provided
3. **Organized output** - Label everything clearly
4. **Detailed analysis** - Push LLM for specific evidence
5. **Actionable results** - Get copy-paste ready remediation

**Time Investment:**
- Initial setup and learning: 1-2 hours
- Per-device security assessment: 15-20 minutes
- Per-device OSPF assessment: 20-25 minutes
- Multi-device OSPF troubleshooting: 30-40 minutes

**When to Automate:**
- Regular assessments (weekly/monthly)
- Large number of devices (>10)
- Standardized reporting requirements
- Integration with ticketing/monitoring systems

---

## Appendix: Additional Resources

### Recommended LLM Models for This Task

**Tier 1 (Best Results):**
- Claude 3.5 Sonnet or higher
- GPT-4 Turbo or higher
- Gemini 1.5 Pro

**Tier 2 (Good Results):**
- GPT-4
- Claude 3 Opus
- Llama 3 70B+

**Tier 3 (Basic Results):**
- GPT-3.5 Turbo
- Mistral Large
- Llama 3 8B

### Useful Cisco Documentation Links

- NSA Router Security Configuration Guide
- NIST SP 800-53 Rev 5
- CIS Cisco IOS Benchmark
- Cisco IOS Security Configuration Guide
- Cisco OSPF Design Guide
- Cisco OSPF Troubleshooting Guide

### Training Resources

- Cisco IOS CLI basics
- OSPF fundamentals
- Network security principles
- Markdown syntax

---

**Document Version:** 1.0
**Last Updated:** 2026-01-12
**Maintained By:** NetJarvis Team
**Feedback:** [Document issues or improvements]

---

*This document is designed to be printed, shared, and used as a field guide for network engineers performing manual assessments with AI assistance.*
