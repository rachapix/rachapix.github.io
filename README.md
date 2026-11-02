# RachaPix

Bill splitting and shared-expense tracking for Brazil, with ready-to-pay Pix codes. No accounts, no sign-up.

**Live:** [rachapix.github.io](https://rachapix.github.io/) · **Backend:** [RachaPixAPI](https://github.com/ViniciusGNunes/RachaPixAPI)

## What it does

**Split a bill**
- Enter a total, the participants, and a Pix key. Split equally or by custom amounts.
- Participants open a public link, pick their name, and get a Pix QR code and "copia e cola" code with their share filled in.
- The organizer tracks payments on a private admin page. A participant's "I paid" claim only counts after the organizer confirms it.

**Manage group expenses** (a shared house, a trip)
- Log expenses with category, payment method, and payer. Divide them equally, by amount, or by percentage.
- See each member's balance and a minimal list of transfers that settles the group.
- Record settlements and pay a creditor through a generated Pix code.
- Review spending by category, payment method, member, and month.
- Use the same member on several devices with a single-use link code.

## Engineering highlights

- **Pix BR Code generation** on the backend: EMV payload with CRC16 checksum, key-type normalization (CPF, CNPJ, email, phone, random key), and sanitized receiver data.
- **Exact money splitting:** shares are rounded down to cents and leftover cents are assigned so the parts always equal the total.
- **Accountless access control:** secret tokens replace logins. Share links keep the token in the URL fragment so it never reaches the server on page load. Wrong tokens and unknown groups both return `404`, so group IDs cannot be probed.
- **Per-device identity:** device tokens are stored hashed, link codes expire after 5 minutes, and redemption is rate limited.
- **Debt simplification:** a greedy min-cash-flow algorithm produces the suggested transfers.
- **Consistent errors:** RFC 7807 problem details with a correlation ID on every failure.
- **Tested:** xUnit integration tests on the API, Vitest and Testing Library on the frontend.

## Stack

| | |
| --- | --- |
| Frontend | Next.js 16 (static export), React 19, TypeScript, Tailwind CSS 4, TanStack Query, Recharts |
| Backend | ASP.NET Core 9, EF Core 9, PostgreSQL |
| Testing | xUnit, Vitest, Testing Library |
