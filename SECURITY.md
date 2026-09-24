# Security policy

This policy covers every repository in the ThinkWatchProject organization:
ThinkWatch Lite, ThinkWatch Core, ThinkWatch Enterprise (the `ThinkWatch`
repository), the Homebrew tap and the website.

## Reporting a vulnerability

Vulnerabilities are reported privately. Do not describe one in a public
issue, pull request, commit or comment.

- If the affected repository offers **Report a vulnerability** under its
  **Security and quality** tab, use it. The report is visible only to the
  reporter and the maintainers.
- Otherwise, open an issue in the affected repository titled
  **Security contact request**, with nothing else in it: no description of
  the problem, no affected component and no proof of concept. A maintainer
  answers by opening a draft security advisory in that repository and
  adding the reporter to it. A draft advisory is visible only to the
  maintainers and the people added to it; the details, the discussion and
  the fix stay there until the advisory is published.

If it is unclear which repository is affected, any of the three product
repositories will do.

## What to include

- The product and its version. ThinkWatch Lite shows it under
  Settings › About, ThinkWatch Core prints it with `twcore --version`, and
  ThinkWatch Enterprise shows it under Settings › General in the console.
- The operating system, the architecture and the install method.
- The steps to reproduce the problem, and what an attacker gains from it.
- A proof of concept, if there is one, with real keys and tokens removed.

## Fixed versions

A fix ships in the next release of the affected product; earlier releases
are not patched separately. ThinkWatch Lite updates itself (a Homebrew
install follows the cask) and carries its own copy of ThinkWatch Core, so a
fix in Core reaches Lite users through a Lite release. ThinkWatch Core on a
server is upgraded with `twcore upgrade`. ThinkWatch Enterprise is upgraded
by deploying the new release.
