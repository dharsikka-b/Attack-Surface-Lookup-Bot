##Attack Surface Lookup Bot

## Red Team Recon & OSINT Using Shodan InternetDB

An n8n-powered Telegram bot for automated IP reconnaissance and preliminary attack-surface analysis using Shodan InternetDB.

## 1. Introduction

The Attack Surface Lookup Bot is a cybersecurity automation project designed to simplify preliminary IP reconnaissance using Open-Source Intelligence (OSINT). It integrates n8n workflow automation, Telegram, and the Shodan InternetDB API to retrieve available information associated with a public IP address and present it in a structured Telegram message.

The project helps users understand basic network exposure information, including available open ports, hostnames, and CVE references, without manually searching for these details.

## 2. Objectives

- Automate preliminary IP reconnaissance using OSINT.
- Retrieve available information about public IP addresses.
- Identify reported open ports and associated hostnames.
- Display available Common Vulnerabilities and Exposures (CVE) references.
- Validate user input before processing requests.
- Apply a custom port watchlist to classify lookup results.
- Deliver readable reconnaissance summaries through Telegram.

## 3. Technologies Used

| Technology | Purpose |
|---|---|
| n8n | Workflow automation and integration |
| Telegram Bot API | Receives user commands and sends reports |
| Shodan InternetDB API | Retrieves available IP-related information |
| JavaScript | Processes returned data and checks watchlist ports |
| AI Agent | Helps organize the response, when configured |
| Structured Output Parser | Structures AI output, when configured |

## 4. Key Features

- **Automated IP Lookup:** Retrieves available InternetDB information for a submitted public IP address.
- **Input Validation:** Checks user input and rejects invalid requests.
- **Port Information:** Displays available open-port information.
- **Hostname Information:** Reports hostnames when returned by the API.
- **CVE References:** Displays available CVE identifiers associated with the lookup.
- **Custom Port Watchlist:** Checks ports 135, 143, and 443.
- **Result Classification:** Applies a custom HIGH/NORMAL label based on watchlist matches.
- **Telegram Integration:** Delivers the formatted result directly to the user.
- **Automated Reporting:** Organizes retrieved information into a readable summary.

## 5. Workflow Architecture

The workflow consists of the following nodes:

1. **Telegram Trigger:** Receives the command sent by the user.
2. **Validate Input:** Checks whether the supplied IP address is valid for processing.
3. **Is Input Valid:** Routes valid requests to the lookup process and rejects invalid input.
4. **HTTP Request:** Queries the Shodan InternetDB API.
5. **JavaScript Processing:** Checks the returned port information against the custom watchlist.
6. **Has InternetDB Data:** Determines whether useful lookup information is available.
7. **AI Agent and Structured Output Parser:** Organize the report output, if configured.
8. **Format Message:** Prepares the response for Telegram.
9. **Redact Secrets:** Applies the configured secret-redaction logic.
10. **Telegram Send Message:** Sends the final report to the user.

## 6. Working Procedure

1. The user sends an IP lookup command to the Telegram bot.
2. The workflow validates the supplied IP address.
3. If the input is invalid or a private IP address is rejected by the validation logic, the workflow returns an invalid-input response.
4. For accepted input, the HTTP Request node queries Shodan InternetDB.
5. The returned data is processed to extract available information.
6. The JavaScript logic checks whether any returned ports match the configured watchlist.
7. The workflow prepares a structured summary using the configured processing and formatting nodes.
8. The completed message is sent back to the user through Telegram.

## 7. Example Usage

Send the following command to the configured Telegram bot:

`/ip 1.1.1.1`

The bot attempts to retrieve available information for the supplied IP address and returns a formatted reconnaissance summary.

Example command with invalid input:

`/ip hello`

Expected result: An invalid-input response.

Example command with a private IP address:

`/ip 192.168.1.1`

Expected result: An invalid-input response, according to the configured validation logic.

## 8. Custom Port Watchlist

The project uses a predefined watchlist containing the following ports:

| Port | Common Association |
|---|---|
| 135 | Microsoft RPC services |
| 143 | IMAP email service |
| 443 | HTTPS service |

The JavaScript logic applies a simplified classification rule:

- **HIGH:** At least one returned port matches the configured watchlist.
- **NORMAL:** No returned ports match the configured watchlist.

**Important:** These labels represent the project's custom rule, not a verified vulnerability severity assessment. A matching port does not automatically mean that a service is vulnerable or misconfigured.

## 9. Testing and Results

The following test cases were used to evaluate the workflow.

| Test Case | Input | Expected Behaviour |
|---|---|---|
| Valid public IP | `/ip 1.1.1.1` | Retrieve available InternetDB data and return a formatted response |
| Invalid input | `/ip hello` | Reject the request |
| Private IP | `/ip 192.168.1.1` | Reject the request under the configured validation rules |
| Watchlist match | Public IP with a returned watchlist port | Apply the HIGH classification rule |
| No watchlist match | Public IP without a returned watchlist port | Apply the NORMAL classification rule |

Actual lookup results depend on the information available through InternetDB. The workflow's output and classification should be verified through testing.

## 10. Setup Instructions

### Prerequisites

- An n8n instance.
- A Telegram account.
- A Telegram bot created using BotFather.
- Internet connectivity to access the Shodan InternetDB API.
- AI model credentials if the AI Agent requires them.

### Installation and Configuration

1. Create or open an n8n instance.
2. Import the exported workflow JSON file, when available.
3. Configure the Telegram credentials securely within n8n.
4. Verify the HTTP Request endpoint and input-processing logic.
5. Review the JavaScript watchlist and classification rules.
6. Configure the AI Agent and Structured Output Parser if they are part of the workflow.
7. Verify the message-formatting and secret-redaction steps.
8. Execute the workflow and test it using appropriate public IP addresses.
9. Confirm that the expected response is delivered through Telegram before enabling the workflow for regular use.

### InternetDB API Endpoint

The workflow uses the following endpoint format:

`https://internetdb.shodan.io/<IP_ADDRESS>`

Replace `<IP_ADDRESS>` with the IP address being queried. The service returns information only when data is available for the requested address.

## 11. Security Considerations

- Use the project only for authorized security research and legitimate reconnaissance.
- Do not upload Telegram bot tokens, API keys, passwords, or other credentials to a public repository.
- Store credentials securely using n8n's credential-management features.
- Review workflow exports for embedded secrets before sharing them.
- Treat external reconnaissance data as informational rather than conclusive evidence.
- Verify findings before making security decisions.

## 12. Limitations

- InternetDB may not contain information for every IP address.
- Returned information may be incomplete or outdated.
- A listed port does not prove that a vulnerability exists.
- CVE references do not independently prove that a target is exploitable.
- The HIGH/NORMAL classification is based on a simplified custom watchlist.
- The quality of AI-generated summaries and recommendations depends on the configuration and available input data.
- The project is intended for preliminary reconnaissance and does not replace a comprehensive vulnerability assessment.

## 13. Project Report

The project documentation is available in this repository as `Red Team Recon.pdf`.

The report describes the project's objectives, technologies, workflow, testing, cybersecurity concepts, and limitations.

## 14. Future Enhancements

- Add configurable port watchlists.
- Improve error handling for API failures and missing data.
- Add more detailed logging and execution monitoring.
- Improve report formatting and severity explanations.
- Add additional validation and automated test cases.
- Improve secret-redaction verification.

## 15. Conclusion

The Attack Surface Lookup Bot demonstrates how workflow automation, OSINT, API integration, and Telegram can be combined to support preliminary IP reconnaissance. By automating data retrieval, applying a custom port watchlist, and returning structured summaries, the project provides a simple educational example of cybersecurity automation.

The results should be interpreted carefully because available data may be incomplete, and a watchlist match is not proof of a vulnerability.

## Author

**Dharsikka B**

Cybersecurity Automation Project
