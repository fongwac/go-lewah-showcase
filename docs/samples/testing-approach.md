# Sample — Testing approach

*A documentation sample: how each development phase is verified before it is accepted.*

## Rules

- Tests never run against production or live data. Each suite works on a **fresh copy** of a development database
  that contains synthetic schools and people only, with its own temporary test server.
- Every phase ends with a written completion report. The report separates what is implemented and tested from what
  is only prepared, and lists every failure with its cause.
- Fixes to existing behaviour that were not requested are reported rather than made silently.

## Levels

1. **API and security tests.** Every permission rule is exercised, including the refusals. Examples:
   - a user from another school gets 404 or 403;
   - a Studio Producer cannot grant themselves extra rights;
   - a student cannot speak without recorded parental consent;
   - review rights alone do not allow broadcasting.
2. **Browser tests.** Real browser sessions run with synthetic accounts at desktop, tablet and phone widths, in
   English and French. They check that:
   - the right controls appear for each role;
   - the page has no horizontal overflow;
   - no script errors occur.
3. **Real-audio tests for live radio.** Each simulated participant's microphone plays a distinct tone. The audio a
   listener actually receives is analysed frequency by frequency, which proves who is audible. For example, a muted
   or paused speaker is silent for everyone, and lifting a pause does not switch their microphone back on. The media
   server's own participant list confirms who may publish.
4. **Regression.** All earlier suites are re-run before a phase is accepted.

## Honest limits

Browser and simulated-microphone tests are not the same as tests on real phones over real networks. Real-device
testing is tracked as a separate, pending item.
