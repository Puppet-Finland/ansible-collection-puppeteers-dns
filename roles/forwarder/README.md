# puppeteers.dns.forwarder

This is a simple Ansible role for configuring a Bind DNS server in a simple
forwarder role. This can be useful if you, for example, want VPN clients to be
able to resolve DNS records from a Cloud provider's private DNS, such as Amazon
Route 53 private hosted zone or Azure Private DNS zone.

# Usage

Include the role puppeteers.dns.forwarder. You need to define which IPs/IP
ranges to allow queries from:

    puppeteers_dns_forwarder_allow_query: "10.0.0.0/8; localhost;"

You also need to define the servers requests are forwarded to:

    puppeteers_dns_forwarder_forwarders: "1.2.3.4; 5.6.7.8;"

This parameter is in the usual bind format (not a list) and doubles as
the allowlist for recursion.

If you wish to disable DNSSec validation for one or more domains (and their
subdomains, if any):

    puppeteers_dns_forwarder_unvalidated_domains:
      - acme.org
      - app.mydomain.com

To enable serving stale (TTL expired) DNS records in case upstream server DNS
servers are temporarily unreachable:

    puppeteers_dns_forwarder_stale_cache_enable: "yes"
    puppeteers_dns_forwarder_stale_answers_enable: "yes"
    puppeteers_dns_forwarder_stale_refresh_time: 30s
    puppeteers_dns_forwarder_stale_answer_ttl: 30s

Make sure to surround the enable parameters with quotes or Ansible will convert
them into booleans and variable validation will fail. See [this
table](https://support.huaweicloud.com/intl/en-us/bestpractice-dns/dns_bestprac_0011.html#section3)
for details on the stale parameters.

If you wish to poke a hole in firewalld use:

    puppeteers_dns_forwarder_manage_firewalld: true
    puppeteers_dns_forwarder_firewalld_zone: public

See [tasks/main.yml](tasks/main.yml) for a full list of parameters.
