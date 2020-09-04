---
layout: post
title: Use subdomain for receiving email at mailgun
date: '2015-10-04 20:19:46'
---

I [previously set up mailgun](/ghost-and-digitalocean/) for sending email from this ghost site and therefore from the `dot.kitchen` domain. However I now want to receive emails on that domain, so as recommended at mailgun, I'm switching to using a subdomain  of `mg.dot.kitchen` instead of the top level `dot.kitchen` domain. Following that I'll be able to [set up some mailboxes](/using-zoho-mail-to-host-dot-kitchen-email) for `dot.kitchen` in [Zoho](https://www.zoho.com/mail/).

## Adjusting setup
1. Add new domain in mailgun of `mg.dot.kitchen`
2. Update DNS records as specified by mailgun. This essentially meant updating the previous set of records to incorporate the `mg.` prefix.
3. Wait 20 minutes for DNS to propogate
4. Verify domain
5. Test receiving mail at e.g. test@mg.dot.kitchen (requires route to be set up - catch all one is fine e.g. using `catch_all()` and `store()`)
6. Update ghost's config.js with new settings
7. Restart ghost `service ghost restart`
8. Test email with settings > labs > send a test email