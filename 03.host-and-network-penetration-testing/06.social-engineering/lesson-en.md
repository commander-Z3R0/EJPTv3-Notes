# Social Engineering

## What Is Social Engineering?

Social engineering is a set of techniques used to manipulate people into revealing confidential information, performing unsafe actions, or making decisions that weaken security.

Instead of exploiting only technical vulnerabilities, social engineering takes advantage of:

- Trust.
- Authority.
- Fear.
- Curiosity.
- Urgency.
- Greed.
- Helpfulness.
- Familiarity.

The attacker creates a believable situation, known as a **pretext**, and tries to influence the victim’s behavior.

A social engineering attack may attempt to:

- Obtain usernames or passwords.
- Make someone open a malicious link or attachment.
- Convince an employee to transfer money.
- Obtain access to a building or restricted area.
- Make a user install unauthorized software.
- Collect information that can be used in a later attack.

---

## Common Social Engineering Techniques

### Phishing

Phishing uses fraudulent emails or online messages that appear to come from a trusted source.

The objective may be to make the victim:

- Click a malicious link.
- Open an attachment.
- Submit credentials.
- Download malware.
- Transfer money.
- Reply with sensitive information.

### Pretexting

Pretexting involves creating a fictional scenario to obtain information or influence an action.

Examples include pretending to be:

- An IT support employee.
- A bank representative.
- A supplier.
- A manager.
- An auditor.
- A delivery company.
- A new employee.

The attacker usually creates a believable story and uses it to justify the request.

### Baiting

Baiting offers something attractive to encourage the victim to perform an unsafe action.

Examples include:

- A free software download.
- A fake document.
- A USB drive labelled with interesting information.
- A fake prize.
- A supposed salary or invoice document.

### Impersonation

Impersonation occurs when an attacker pretends to be someone trusted by the victim.

The attacker may copy:

- A person’s name.
- Email signature.
- Job title.
- Company logo.
- Writing style.
- Phone number or caller ID.

### Quid Pro Quo

Quid pro quo means offering something in exchange for information or assistance.

Examples include:

- Offering technical support in exchange for a password.
- Promising a reward for completing a form.
- Offering access to a service after collecting personal information.

### Emotional Pull

The attacker tries to create sympathy, trust, fear, or another emotional reaction.

For example, the attacker may pretend to be:

- A colleague in an emergency.
- A manager who needs immediate help.
- A family member.
- A customer facing a serious problem.

### Urgency

Urgency pressures the victim to act quickly without verifying the request.

Common examples include:

- “Your account will be disabled today.”
- “The invoice must be paid immediately.”
- “The CEO needs this file within five minutes.”
- “A suspicious login was detected.”

### Blackmail And Extortion

The attacker threatens to publish or reveal private, embarrassing, or damaging information unless the victim complies.

### Watering Hole

A watering-hole attack compromises a website or online resource frequently visited by a specific group.

The attacker uses the trust users already have in that website to expose them to malicious content or collect information.

### Physical Access

Physical social engineering uses direct interaction with people, buildings, or devices.

Examples include:

- Pretending to be a maintenance worker.
- Following an employee through a secured door.
- Using a stolen or cloned badge.
- Searching discarded documents.
- Leaving a malicious USB device in a public area.
- Taking advantage of an unlocked workstation.

---

## Phishing

Phishing is one of the most common forms of social engineering.

A phishing message usually tries to appear legitimate by imitating a trusted organization, employee, or service.

### Common phishing indicators

- Unexpected requests for credentials.
- Links with unusual domains.
- Slightly modified company names.
- Spelling or grammar mistakes.
- Unexpected attachments.
- A strong sense of urgency.
- Requests to bypass normal procedures.
- Mismatched sender and reply-to addresses.
- Requests for payment or sensitive data.
- Login pages reached through an email link.

---

## Types Of Phishing

### Spear Phishing

Spear phishing is a targeted attack against a specific person or group.

The attacker researches the target and personalizes the message using information such as:

- Name.
- Job title.
- Company.
- Current projects.
- Suppliers.
- Public social media information.

### Whaling

Whaling is spear phishing aimed at high-profile targets, such as:

- CEOs.
- CFOs.
- Directors.
- Administrators.
- Security managers.
- Executives with financial authority.

### Smishing

Smishing is phishing performed through SMS or messaging applications.

The message may contain:

- A fake delivery notification.
- A banking alert.
- A payment request.
- A shortened URL.
- A fake account-verification message.

### Vishing

Vishing uses voice calls to manipulate the victim.

The attacker may use:

- Caller ID spoofing.
- Impersonation.
- Technical support scenarios.
- Fake fraud alerts.
- Payment requests.
- Requests for verification codes.

### Business Email Compromise

Business Email Compromise, or BEC, uses compromised or spoofed business email accounts to convince employees to:

- Transfer money.
- Change supplier bank details.
- Send confidential documents.
- Buy gift cards.
- Reveal internal information.

---

## Physical Access And Rubber Ducky

Physical access means interacting directly with a device, workstation, server, building, or restricted area.

Physical access can bypass many remote security controls because the attacker may be able to interact directly with the hardware.

### Rubber Ducky

A Rubber Ducky is a USB device that identifies itself as a keyboard and automatically types pre-programmed keystrokes.

It can be abused to:

- Open a terminal.
- Execute commands.
- Download files.
- Modify settings.
- Launch applications.

The main risk is that users may trust USB devices and connect them without verifying their origin.

### Defensive controls

- Block unauthorized USB devices.
- Disable automatic execution.
- Restrict local administrator privileges.
- Use endpoint detection tools.
- Train users never to connect unknown USB devices.
- Protect workstations when users are away.
- Use physical access controls.

---

## Social Engineering In Penetration Testing

The purpose of social engineering in a penetration test is to identify weaknesses in people, processes, and security controls.

A legitimate assessment should test whether employees:

- Verify unexpected requests.
- Report suspicious messages.
- Follow payment procedures.
- Protect sensitive information.
- Challenge unknown visitors.
- Avoid using unknown USB devices.
- Use multi-factor authentication correctly.

### Required authorization

Before starting a social engineering assessment, obtain written authorization that defines:

- The client and authorized sponsor.
- The target departments or users.
- The allowed communication channels.
- The approved pretexts.
- The test dates and time windows.
- The maximum number of messages or calls.
- Prohibited targets and topics.
- Data-handling requirements.
- Emergency contacts.
- Conditions for stopping the test.

### Safety principles

- Use test accounts and dummy credentials.
- Never collect real passwords.
- Never use real malware.
- Do not create panic or threaten employees.
- Do not target personal accounts.
- Avoid sensitive topics such as health, family emergencies, or personal finances.
- Stop immediately if the test creates operational or personal risk.
- Inform the client’s security team according to the agreed communication plan.

---

## GoPhish

GoPhish is an open-source platform used to simulate phishing campaigns in authorized security-awareness exercises.

It can help security teams measure:

- Email delivery.
- Message opens.
- Link clicks.
- Reports sent by users.
- Interaction with a training page.
- The effectiveness of awareness training.

### Safe usage

For a safe lab campaign:

- Use only test users.
- Use a private lab network.
- Use dummy data.
- Use a landing page that records clicks but does not store passwords.
- Do not imitate real external companies without permission.
- Do not send messages to people outside the approved scope.

---

## Installing GoPhish With Docker

Create a local lab directory:

```bash
mkdir -p ~/gophish-lab  # creates a directory for the GoPhish lab.
cd ~/gophish-lab  # moves into the lab directory.
```

Run the local demonstration container:

```bash
docker run --rm -it -p 3333:3333 gophish/demo  # starts the GoPhish demonstration environment on local port 3333.
```

### Command explanation

- `docker run` starts a container.
- `--rm` removes the container automatically when it stops.
- `-it` provides an interactive terminal.
- `-p 3333:3333` maps the container’s management port to the local machine.
- `gophish/demo` uses the demonstration image.

For a normal lab deployment, download the appropriate release for your operating system, extract it, and run the GoPhish binary.

```bash
chmod +x gophish  # gives execution permission to the GoPhish binary.
./gophish  # starts GoPhish.
```

Read the startup output to identify the local administration address and credentials.

---

## GoPhish Campaign Components

A GoPhish campaign normally includes:

### Sending Profile

Defines how messages are sent.

In a lab, use:

- A local SMTP server.
- A dedicated test mailbox.
- A controlled mail relay.
- A sender identity approved by the client.

### Email Template

Defines the content of the training message.

A safe template should:

- Clearly belong to the approved exercise.
- Avoid collecting real credentials.
- Use a training explanation after the click.
- Contain a reporting instruction.
- Avoid harmful attachments.

### Landing Page

Defines the page shown after the user clicks the link.

For a safe exercise, the landing page should:

- Record only the click.
- Avoid storing passwords.
- Display an educational message.
- Explain the warning signs.
- Provide a method for reporting future phishing attempts.

### Users And Groups

Contains the approved test recipients.

Use:

- Test accounts.
- A limited pilot group.
- Users who have been approved by the client.
- An internal distribution list controlled by the security team.

### Campaign

Connects the sending profile, email template, landing page, and user group.

Before launching, verify:

- The recipients are in scope.
- The message contains no real credential request.
- The landing page does not store sensitive data.
- The campaign time window is approved.
- The SOC or security contact knows how to handle alerts.

---

## Safe GoPhish Workflow

A responsible workflow is:

1. Obtain written authorization.
2. Define the scope and safety limits.
3. Build a private lab or controlled test environment.
4. Create test users and a dummy email domain.
5. Configure a sending profile using a test SMTP server.
6. Create a training email template.
7. Create a landing page that records only clicks.
8. Add approved users and groups.
9. Launch a small pilot campaign.
10. Review opens, clicks, reports, and user feedback.
11. Stop the campaign at the agreed time.
12. Delete campaign data according to the retention policy.
13. Report findings without exposing individual employees unnecessarily.

---

## How To Defend Against Social Engineering

### User awareness

Training should teach users to:

- Verify unusual requests.
- Avoid clicking unexpected links.
- Inspect domains and sender addresses.
- Report suspicious messages.
- Confirm payment changes using a second channel.
- Never share passwords or MFA codes.
- Avoid unknown USB devices.
- Challenge unexpected visitors.

### Technical controls

Useful controls include:

- Multi-factor authentication.
- Email filtering.
- Attachment sandboxing.
- URL analysis.
- Domain protection.
- DMARC, DKIM, and SPF.
- Endpoint detection and response.
- USB device control.
- Password managers.
- Least-privilege access.
- Conditional access policies.

### Defense in depth

Security should use several independent layers:

- User awareness.
- Email security.
- Identity protection.
- Endpoint protection.
- Network monitoring.
- Physical security.
- Incident-response procedures.

If one layer fails, another layer should detect or limit the attack.

---

## Incident Response To Social Engineering

If a user interacts with a suspicious message:

1. Do not continue interacting with it.
2. Disconnect the device from the network if malware may have executed.
3. Report the message to the security team.
4. Do not delete the email unless instructed.
5. Change credentials if they may have been exposed.
6. Revoke suspicious sessions or tokens.
7. Review MFA activity.
8. Check endpoint and identity logs.
9. Notify affected teams.
10. Document the timeline and actions taken.

---

## Case Studies

The following examples illustrate common social engineering patterns:

- **Google and Facebook fake invoicing:** business email compromise and fraudulent invoices.
- **FACC CEO fraud:** impersonation and executive-payment fraud.
- **Robinhood vishing:** voice-based social engineering and customer-data exposure.
- **Fake Excel file:** malicious attachment used to deliver malware.
- **HTML table with a Windows logo:** visual deception used to make a phishing email appear legitimate.
- **FIN7 USB in the mail:** physical baiting using malicious USB devices.

### Lessons from the cases

- Trust in a brand does not guarantee that a message is legitimate.
- Executive requests should still follow normal approval procedures.
- A familiar logo does not prove authenticity.
- Attachments should be verified through another communication channel.
- Unexpected USB devices should never be connected.
- Financial changes require independent verification.

---

## Key Takeaways

- Social engineering exploits human trust, emotions, and decision-making.
- Phishing, pretexting, impersonation, baiting, vishing, and smishing are common techniques.
- Spear phishing targets specific people, while whaling targets executives.
- Physical security is part of cybersecurity.
- GoPhish is useful for authorized awareness simulations.
- Never collect real passwords or use malicious payloads during a standard awareness exercise.
- Written authorization, clear scope, safety limits, and data protection are essential.
- The best defense combines user training, technical controls, physical security, and incident response.
