# Automation Evidence

## Connector Configuration

![osTicket Connector Configuration](osticket-connector-configuration.png)

Shows the webhook connector name, POST method, osTicket endpoint, and enabled HTTP-header configuration. The credential value is not visible.

## Connector Test

![osTicket Connector Test](osticket-connector-test-success.png)

Shows a successful connector test at the time of capture.

## API-Created Tickets

![osTicket Ticket List](osticket-ticket-list.png)

Shows tickets for both custom rule names.

![Historically Named Windows Rule Ticket](rdp-api-ticket.png)

![SSH Rule Ticket](ssh-api-ticket.png)

The individual tickets show API source and the basic body `Investigate Rule: <rule name>`. They do not show enriched alert fields, assignment automation, response actions, or case closure.

The Windows ticket inherits the historical rule name; it does not prove the underlying rule was RDP-specific.
