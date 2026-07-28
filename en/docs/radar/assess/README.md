# Assessment Procedure

This directory contains radar entries for tools that are interesting and
worth keeping an eye on but have not yet been trialled in SAT.

## What assess means

A tool in assess/ has been identified as potentially useful for SAT but
has not been tested against real SAT archives or integrated into the SAT
toolchain. 

Do your own research before adopting any tool from this directory.

## Before moving a tool from assess/

All of the following must be true:

- [ ] Radar entry is complete with no empty sections
- [ ] Security assessment has been completed and documented
- [ ] Assessment status is `passed` or `partial` with known limitations noted
- [ ] Licence is confirmed compatible with CC BY-SA 4.0
- [ ] Tool has been installed and run against at least one SAT archive
- [ ] A brief test report has been added to the status notes section
- [ ] At least one team member has reviewed the entry

## Security assessment procedure

Every tool that touches archive content must be assessed for network and
file system behaviour before being used in a SAT production environment.

### Linux

```bash
# Monitor network connections while using the tool
sudo nethogs

# Or monitor file system activity
strace -e trace=file <command> 2>&1 | grep -v /proc
```

### macOS

```bash
# Network connections
lsof -i -n -P | grep <process-name>

# File system activity
fs_usage <process-name>
```

### Windows

```powershell
# Use Wireshark for network monitoring
# Use Process Monitor (Sysinternals) for file system activity
```

### What to look for

**Network — flag if any of these are found:**

- Outbound connections during normal editing or use
- Connections to analytics or telemetry endpoints
- Connections other than explicit update checks that can be disabled
- Any transmission of file paths, filenames, or content

**File system — flag if any of these are found:**

- Files created inside archive directories without user action
- Content cached outside the working directory
- Recent file history stored in a location that could leak archive paths

### Recording results

Update the security assessment section of the radar entry with:

- Tool version tested
- Platform tested on
- Date of assessment
- Method used
- What was found
- Assessment status: `passed`, `partial`, or `failed`

## Licence compatibility

SAT tools are licenced under GPL v3.
SAT's documentation archive content is licenced under CC BY-SA 4.0.

SAT's documentation archive should NEVER EVER includes private, secret or sensitive information

Dependencies and tools must be compatible with the appropriate licence depending on what they touch.

### SAT tools compatibility (GPL v3)

| Licence     | Compatible   | Notes                                         |
|-------------|--------------|-----------------------------------------------|
| GPL v3      | yes          |                                               |
| GPL v2      | partial      | GPL v2 only is incompatible with GPL v3       |
| AGPL v3     | yes          | AGPL v3 is compatible with GPL v3             |
| LGPL v2/v3  | yes          | permitted to link with GPL v3                 |
| Apache 2.0  | yes          | compatible with GPL v3 since Apache 2.0 v2006 |
| MIT         | yes          |                                               |
| BSD 2/3     | yes          |                                               |
| MPL 2.0     | yes          | file-level copyleft, compatible with GPL v3   |
| Proprietary | no           | incompatible with GPL v3                      |

### SAT documentation and archive content compatibility (CC BY-SA 4.0)

| Licence          | Compatible   | Notes                                    |
|------------------|--------------|------------------------------------------|
| CC BY-SA 4.0     | yes          |                                          |
| CC BY 4.0        | yes          | less restrictive, compatible             |
| CC BY-SA 3.0     | partial      | older version, check case by case        |
| CC0              | yes          | public domain, no restrictions           |
| GPL v3           | no           | software licence, not for content        |
| MIT              | no           | software licence, not for content        |
| All Rights Reserved | no        | incompatible with open archive goals     |

### The distinction in practice

A tool that only processes content without being distributed alongside
SAT (for example a formatter run from the command line) is assessed
against the GPL v3 column.

A tool whose output is embedded in SAT documentation or archive content
is assessed against the CC BY-SA 4.0 column.

When in doubt consult `legal/COMPLIANCE.md`.

## Archive content licencing

SAT does not prescribe a licence for archive content. That is the archive
owner's decision.

If you are unsure where to start, the following resources are widely used
and well documented:

**For creative content, documentation, and writing:**
- [Creative Commons licence chooser](https://creativecommons.org/choose/)
  helps you select the right CC licence for your needs

**For software:**
- [Choose a Licence](https://choosealicense.com) — maintained by GitHub,
  clear plain-language explanations of the most common software licences

**For open data:**
- [Open Data Commons](https://opendatacommons.org/licenses/) — licences
  specifically designed for datasets and databases

The only requirement SAT places on archive content licences is that they
are declared explicitly in the archive definition and in the
`default-canonical-metadata.yml` for that archive.
