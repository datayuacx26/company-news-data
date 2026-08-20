---
schema_version: "1.0.0"
document_id: "a02e6d451762fccec9790459e4ba5fac3aef1decbebca552d23de94f0676077c"
company_key: "yc-sendblue"
company: "Sendblue"
source_id: "yc-sendblue-news-import-cbfb84d5bb49"
canonical_url: "https://www.sendblue.com/blog/appointment-reminder-software-imessage"
published_at: "2026-03-29T00:00:00+00:00"
first_seen_at: "2026-07-24T01:08:51.497224+00:00"
fetched_at: "2026-07-28T22:17:07.377837+00:00"
content_hash: "sha256:4e57afef1d71560c86e4571c01ca4750a00bd8a1434b5f39926ee7f10cdafc98"
---

# Appointment Reminder Software: Why iMessage Gets 98% Open Rates

Setting up automated iMessage appointment reminders with Sendblue involves three components:


**1. Your booking system** (Calendly, Acuity, your EHR, or a custom system) creates an appointment.


**2. Your automation layer** (Zapier, Make, n8n, or direct API integration) triggers reminders at specified intervals before the appointment.


**3. Sendblue's API** sends the iMessage and delivers responses back to your system via webhook.


` // Send appointment reminder 24 hours before async function sendAppointmentReminder(appointment) { const reminderTime = new Date(appointment.dateTime); reminderTime.setHours(reminderTime.getHours() - 24); await sendblue.sendMessage({ number: appointment.patientPhone, content: \[ \`Hi ${appointment.patientName}, this is a reminder for your appointment:\`, \`\`, \`Date: ${appointment.date}\`, \`Time: ${appointment.time}\`, \`Provider: ${appointment.providerName}\`, \`Location: ${appointment.location}\`, \`\`, \`Reply YES to confirm, RESCHEDULE to change, or CANCEL.\`, \].join('\\n'), sendAt: reminderTime.toISOString(), }); } // Handle patient responses via webhook app.post('/webhook/receive', async (req, res) => { const { from_number, content } = req.body; const response = content.trim().toUpperCase(); if (response === 'YES') { await confirmAppointment(from_number); await sendblue.sendMessage({ number: from_number, content: 'Your appointment is confirmed. See you then!', }); } else if (response === 'RESCHEDULE') { await sendblue.sendMessage({ number: from_number, content: 'No problem! Here is a link to reschedule: \[scheduling link\]', }); } else if (response === 'CANCEL') { await cancelAppointment(from_number); await sendblue.sendMessage({ number: from_number, content: 'Your appointment has been cancelled. Reply anytime to rebook.', }); } res.status(200).json({ received: true }); });`
