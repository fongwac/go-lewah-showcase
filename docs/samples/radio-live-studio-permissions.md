# Sample — Go-Lewah Radio: roles and permissions in the Live Studio

*A documentation sample: how one feature's rules were specified. Simplified for this showcase.*

## Principle

Everyone who works on a broadcast uses **one shared Live Studio**. The layout is the same for everyone, but a
control is enabled only if the server says that person may use it. The server checks the permission again when the
control is used. Opening the studio never starts a broadcast.

## Roles

| Role | Who | Scope |
|---|---|---|
| School Super Admin | The school's head | Whole station, ultimate authority |
| Communication Admin | A teacher or administrator appointed by the Super Admin | Only the duties the Super Admin delegates |
| Programme Anchor | Lead presenter of one programme | That programme only |
| Studio Producer | Runs the broadcast technically; may work without a microphone | That programme only |
| Co-host | Presenter on a host microphone (max. 3 hosts, the Producer included if they speak) | That programme only |
| Reporter | Teacher or student with a topic | Speaks only when brought on air |
| Approved speaker | A listener whose request to speak was approved | That broadcast only |

## Who may do what (simplified)

| Action | Super Admin | Comm. Admin | Producer | Anchor | Reporter / speaker |
|---|---|---|---|---|---|
| Open the studio | ✔ | with "monitor" duty | own programme | own programme | own programme |
| Start a broadcast | ✔ (on the school's behalf) | with "start broadcasts" duty | ✔ | if also Producer | ✖ |
| End a broadcast | ✔ | with "monitor" duty | ✔ | if also Producer | ✖ |
| Bring a reporter on air / back | ✔ | with "moderate" duty | ✔ | ✔ | ✖ |
| Pause, mute, revoke a speaker | ✔ | with "moderate" duty | ✔ | ✔ | ✖ |
| Mute or pause a host | ✔ | with "moderate" duty | ✖ | ✖ | ✖ |
| Live school announcement | ✔ | with "start broadcasts" duty | ✖ | ✖ | ✖ |
| Urgent announcement | ✔ | with "emergency" duty | ✖ | ✖ | ✖ |

The "review programmes" duty on its own never grants live broadcasting or emergency rights.

## Moderation, distinguished

| Action | Effect | Ends |
|---|---|---|
| Self-mute | The person switches their own microphone off | When they switch it on |
| Mute | The media server withdraws the right to publish audio | When a moderator lifts it |
| Pause | Same, for 1–15 minutes, without disconnecting | Automatically |
| Revoke | Back to listening | A new request to speak is needed |
| Remove | Disconnected | Refused for the rest of the broadcast |

Lifting a mute or a pause **never switches anyone's microphone on**. The person decides when to speak again.

## Child protection

- A student may speak (as host, reporter or approved speaker) only with **recorded parental permission**. This is
  checked at approval and again when they join.
- Vendors never hear programmes in which students speak, and never see student names.
- Every significant action is recorded in the school's audit history.
