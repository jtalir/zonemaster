# Specification of test zones for CONSISTENCY05


## Table of contents

* [Background](#background)
* [Test Case](#test-case)
* [Test scenarios](#test-scenarios)
* [Test zone names](#test-zone-names)
* [Test scenarios and message tags](#test-scenarios-and-message-tags)
* [Zone setup for test scenarios]


## Background

See the [test zone README file].


## Test Case
This document specifies defined test zones for test case [CONSISTENCY05].


## Test scenarios

The purpose of the test scenarios is to cover all reasonable contexts where
different message tags are outputted when [CONSISTENCY05] is run on a test zone.
The message tags are defined in the test case ([CONSISTENCY05]) and the scenarios
are defined below.

The test scenarios are structured as stated in the [test zone README file].

## Test zone names

The test zone for each test scenario in this document is a subdomain delegated
from the base name (`consistency05.xa`) and that subdomain having the same name as the
scenario. The names of those zones are given in section
"[Zone setup for test scenarios]" below.


## Test scenarios and message tags

If a message tag is not listed for the scenario, its presence or non-presence is
irrelevant to the test scenario and must be ignored.

Scenario name             | Mandatory message tag            | Forbidden message tags
:-------------------------|:---------------------------------|:-------------------------------------------
ADDRESSES-MATCH-1         | ADDRESSES_MATCH                  | IN_BAILIWICK_ADDR_MISMATCH, OUT_OF_BAILIWICK_ADDR_MISMATCH, EXTRA_ADDRESS_CHILD, CHILD_ZONE_LAME, CHILD_NS_FAILED, NO_RESPONSE
ADDRESSES-MATCH-2         | ADDRESSES_MATCH                  | IN_BAILIWICK_ADDR_MISMATCH, OUT_OF_BAILIWICK_ADDR_MISMATCH, EXTRA_ADDRESS_CHILD, CHILD_ZONE_LAME, CHILD_NS_FAILED, NO_RESPONSE
ADDRESSES-MATCH-3         | ADDRESSES_MATCH, CHILD_NS_FAILED | IN_BAILIWICK_ADDR_MISMATCH, OUT_OF_BAILIWICK_ADDR_MISMATCH, EXTRA_ADDRESS_CHILD, CHILD_ZONE_LAME, NO_RESPONSE
ADDRESSES-MATCH-4         | ADDRESSES_MATCH, CHILD_NS_FAILED | IN_BAILIWICK_ADDR_MISMATCH, OUT_OF_BAILIWICK_ADDR_MISMATCH, EXTRA_ADDRESS_CHILD, CHILD_ZONE_LAME, NO_RESPONSE
ADDRESSES-MATCH-5         | ADDRESSES_MATCH, NO_RESPONSE     | IN_BAILIWICK_ADDR_MISMATCH, OUT_OF_BAILIWICK_ADDR_MISMATCH, EXTRA_ADDRESS_CHILD, CHILD_ZONE_LAME, CHILD_NS_FAILED
ADDRESSES-MATCH-6         | ADDRESSES_MATCH                  | IN_BAILIWICK_ADDR_MISMATCH, OUT_OF_BAILIWICK_ADDR_MISMATCH, EXTRA_ADDRESS_CHILD, CHILD_ZONE_LAME, CHILD_NS_FAILED, NO_RESPONSE
ADDRESSES-MATCH-7         | ADDRESSES_MATCH                  | IN_BAILIWICK_ADDR_MISMATCH, OUT_OF_BAILIWICK_ADDR_MISMATCH, EXTRA_ADDRESS_CHILD, CHILD_ZONE_LAME, CHILD_NS_FAILED, NO_RESPONSE
ADDR-MATCH-DEL-UNDEL-1    | ADDRESSES_MATCH                  | IN_BAILIWICK_ADDR_MISMATCH, OUT_OF_BAILIWICK_ADDR_MISMATCH, EXTRA_ADDRESS_CHILD, CHILD_ZONE_LAME, CHILD_NS_FAILED, NO_RESPONSE
ADDR-MATCH-DEL-UNDEL-2    | ADDRESSES_MATCH                  | IN_BAILIWICK_ADDR_MISMATCH, OUT_OF_BAILIWICK_ADDR_MISMATCH, EXTRA_ADDRESS_CHILD, CHILD_ZONE_LAME, CHILD_NS_FAILED, NO_RESPONSE
ADDR-MATCH-NO-DEL-UNDEL-1 | ADDRESSES_MATCH                  | IN_BAILIWICK_ADDR_MISMATCH, OUT_OF_BAILIWICK_ADDR_MISMATCH, EXTRA_ADDRESS_CHILD, CHILD_ZONE_LAME, CHILD_NS_FAILED, NO_RESPONSE
ADDR-MATCH-NO-DEL-UNDEL-2 | ADDRESSES_MATCH                  | IN_BAILIWICK_ADDR_MISMATCH, OUT_OF_BAILIWICK_ADDR_MISMATCH, EXTRA_ADDRESS_CHILD, CHILD_ZONE_LAME, CHILD_NS_FAILED, NO_RESPONSE
CHILD-ZONE-LAME-1         | CHILD_ZONE_LAME, NO_RESPONSE     | IN_BAILIWICK_ADDR_MISMATCH, OUT_OF_BAILIWICK_ADDR_MISMATCH, EXTRA_ADDRESS_CHILD, CHILD_NS_FAILED, ADDRESSES_MATCH
CHILD-ZONE-LAME-2         | CHILD_ZONE_LAME, CHILD_NS_FAILED | IN_BAILIWICK_ADDR_MISMATCH, OUT_OF_BAILIWICK_ADDR_MISMATCH, EXTRA_ADDRESS_CHILD, ADDRESSES_MATCH, NO_RESPONSE
IB-ADDR-MISMATCH-1        | IN_BAILIWICK_ADDR_MISMATCH, EXTRA_ADDRESS_CHILD | OUT_OF_BAILIWICK_ADDR_MISMATCH, CHILD_ZONE_LAME, CHILD_NS_FAILED, NO_RESPONSE, ADDRESSES_MATCH
IB-ADDR-MISMATCH-2        | IN_BAILIWICK_ADDR_MISMATCH       | OUT_OF_BAILIWICK_ADDR_MISMATCH, EXTRA_ADDRESS_CHILD, CHILD_ZONE_LAME, CHILD_NS_FAILED, NO_RESPONSE, ADDRESSES_MATCH
IB-ADDR-MISMATCH-3        | IN_BAILIWICK_ADDR_MISMATCH, NO_RESPONSE | OUT_OF_BAILIWICK_ADDR_MISMATCH, EXTRA_ADDRESS_CHILD, CHILD_ZONE_LAME, CHILD_NS_FAILED, NO_RESPONSE, ADDRESSES_MATCH
IB-ADDR-MISMATCH-4        | IN_BAILIWICK_ADDR_MISMATCH       | OUT_OF_BAILIWICK_ADDR_MISMATCH, EXTRA_ADDRESS_CHILD, CHILD_ZONE_LAME, CHILD_NS_FAILED, NO_RESPONSE, ADDRESSES_MATCH
EXTRA-ADDRESS-CHILD       | EXTRA_ADDRESS_CHILD              | IN_BAILIWICK_ADDR_MISMATCH, OUT_OF_BAILIWICK_ADDR_MISMATCH, CHILD_ZONE_LAME, CHILD_NS_FAILED, NO_RESPONSE, ADDRESSES_MATCH
OOB-ADDR-MISMATCH         | OUT_OF_BAILIWICK_ADDR_MISMATCH   | IN_BAILIWICK_ADDR_MISMATCH, EXTRA_ADDRESS_CHILD, CHILD_ZONE_LAME, CHILD_NS_FAILED, NO_RESPONSE, ADDRESSES_MATCH


## Zone setup for test scenarios

Assumptions for the scenario specifications unless otherwise specified for
the specific scenario:
* For each scenario zone there are two name servers configured.
  * Both NS (ns1 and ns2) are equal in delegation and in zone.
  * Both NS are in-bailiwick
  * Both NS have both IPv4 and IPv6 addresses
  * All required glue are present in the delegation.
  * All glue exactly matches the authoritative address records in correct
    zone (not more and not less records).
  * All NS IP addresses respond with identical zone content.
* Responds with a A record for the zone on query for A.
* Responds with a AAAA record for the zone on query for AAAA.
* All responses are authoritative with [RCODE Name] "NoError"
* EDNS, version 0, is included in all responses on queries with EDNS.
* EDNS is not included in responses on queries without EDNS.
* In undelegated data, `IPv4` and `IPv6`, respectively, are placeholders for the
  actual IP addresses used for the scenario. They are to be found where the data
  is specified.
  * If no placeholder is given with the name server name, then no IP address is
    given and might be looked up.
  * The format for undelegated data follow the format used for `zonemaster-cli`
    (after `--ns`).

### ADDRESSES-MATCH-1
The "happy path". Everything is fine.

* Zone: addresses-match-1.consistency05.xa

### ADDRESSES-MATCH-2
Also the "happy path". Out-of-bailiwick NS this time. And no glue.

* Zone: addresses-match-2.consistency05.xa
  * Both ns1 and ns2 are out-of-bailiwick under the xb tree.
  * ns1 is "ns1.addresses-match-2.consistency05.xb"
  * ns2 is "ns2.addresses-match-2.consistency05.xb"
  * Delegation is without glue.
  * The zone has no address records for the NS names
  * The "addresses-match-2.consistency05.xb" zone has a full set of the
    address records for ns1 and ns2.

### ADDRESSES-MATCH-3
One NS does not give AA answer, but else fine.

* Zone: addresses-match-3.consistency05.xa
  * ns1 responds with AA flag unset.

### ADDRESSES-MATCH-4
One NS does give SERVFAIL response, but else fine.

* Zone: addresses-match-4.consistency05.xa
  * ns1 responds with [RCODE Name] "ServFail".

### ADDRESSES-MATCH-5
One NS does not respond, but else fine.

* Zone: addresses-match-5.consistency05.xa
  * ns1 gives no response at all.

### ADDRESSES-MATCH-6
Also "happy path". Out-of-bailiwick NS, but with glue.

* Zone: child.addresses-match-6.consistency05.xa
  * Both ns1 and ns2 are out-of-bailiwick
  * ns1 is "ns1.sibbling.addresses-match-6.consistency05.xa"
  * ns2 is "ns2.sibbling.addresses-match-6.consistency05.xa"
  * Delegation is with glue.
  * The test zone ("child") has no address records for the NS names, but the
    "sibbling" zone has full set of address records.

### ADDRESSES-MATCH-7
Also "happy path". NS in subdomain.

* Zone: addresses-match-7.consistency05.xa
  * ns1 is "ns1.subdomain.addresses-match-7.consistency05.xa."
  * ns2 is "ns2.subdomain.addresses-match-7.consistency05.xa."
  * Delegation is with glue.
  * "subdomain.addresses-match-7.consistency05.xa" is delegated to the same
    ns1 and ns2.
  * ns1 and ns2 are defined with address records in the "subdomain" zone.

### ADDR-MATCH-DEL-UNDEL-1
Also the "happy path". But there is an undelegated zone to be tested.

* Zone: addr-match-del-undel-1.consistency05.xa
  * Delegated zone on ns1 and ns2.
  * Undelegated zone on ns3 and ns4.
  * Delegated zone has neither ns1, ns2, ns3 nor ns4 as address records.
  * Undelegated zone has neither ns1 nor ns2 as an address record, but it
    has both ns3 and ns4 as address records.
  * Undelgated data:
    * ns3.addr-match-del-undel-1.consistency05.xa/IPv4
    * ns3.addr-match-del-undel-1.consistency05.xa/IPv6
    * ns4.addr-match-del-undel-1.consistency05.xa/IPv4
    * ns4.addr-match-del-undel-1.consistency05.xa/IPv6

### ADDR-MATCH-DEL-UNDEL-2
Also the "happy path". But there is an undelegated zone to be tested, and its
NS are out-of-bailiwick.

* Zone: addr-match-del-undel-2.consistency05.xa
  * Delegated zone on ns1 and ns2.
  * Undelegated zone on "ns3.addr-match-del-undel-2.consistency05.xb" and
    "ns4.addr-match-del-undel-2.consistency05.xb".
  * Delegated and undelegated zone, respectively, do not have neither ns1 nor ns2
    as an address record.
  * Undelegated data:
    * ns3.addr-match-del-undel-2.consistency05.xb
    * ns4.addr-match-del-undel-2.consistency05.xb

### ADDR-MATCH-NO-DEL-UNDEL-1
Also the "happy path". No delegation but there is an undelegated zone to be
tested.

* Zone: addr-match-no-del-undel-1.consistency05.xa
  * No delegated zone.
  * Undelegated zone on ns1 and ns2.
  * Undelegated data:
    * ns1.addr-match-no-del-undel-1.consistency05.xa/IPv4
    * ns1.addr-match-no-del-undel-1.consistency05.xa/IPv6
    * ns2.addr-match-no-del-undel-1.consistency05.xa/IPv4
    * ns2.addr-match-no-del-undel-1.consistency05.xa/IPv6

### ADDR-MATCH-NO-DEL-UNDEL-2
Also the "happy path". No delegation but there is an undelegated zone to be
tested. NS are out-of-bailiwick.

* Zone: addr-match-no-del-undel-2.consistency05.xa
  * No delegated zone.
  * Undelegated zone on "ns3.addr-match-no-del-undel-2.consistency05.xb" and
    "ns4.addr-match-no-del-undel-2.consistency05.xb".
  * Undelegated data:
    * ns3.addr-match-no-del-undel-2.consistency05.xb
    * ns4.addr-match-no-del-undel-2.consistency05.xb

### CHILD-ZONE-LAME-1
Lame. No NS responds.

* Zone: child-zone-lame-1.consistency05.xa
  * ns1 and ns2 do not respond.

### CHILD-ZONE-LAME-2
Lame. One NS non-AA and one NS SERVFAIL.

* Zone: child-zone-lame-2.consistency05.xa
  * ns1 responses with AA bit unset.
  * ns2 responds with [RCODE Name] "ServFail".

### IB-ADDR-MISMATCH-1
For one NS (in-bailiwick), the addresses in the glue do not match those in the
authoritative data from the zone.

* Zone: ib-addr-mismatch-1.consistency05.xa
  * ns2 is defined in the zone, but with different addresses (IPv4 and IPv6),
    i.e. not the same as in glue.
  * Both ns2 servers (IP address sets from glue and child, respectively) must
    give identical DNS responses.

### IB-ADDR-MISMATCH-2
For one NS (in-bailiwick), address records exist in the glue, but not in the
authoritative data for the zone.

* Zone: ib-addr-mismatch-2.consistency05.xa
  * ns2 is not defined in the zone, i.e. there are no address records for ns2
    (IPv4 or IPv6) in the zone.

### IB-ADDR-MISMATCH-3
For ns2 (in-bailiwick), there is no NS for ns2 and the glue does not match any
address records in the zone. Furthermore, ns2 does not respond.

* Zone: ib-addr-mismatch-3.consistency05.xa
  * There is no NS record with ns2 in RDATA.
  * ns2 is not defined in the zone, i.e. there are no address records for ns2
    (IPv4 or IPv6) in the zone.
  * ns2 does not respond (but it is in the delegation)

### IB-ADDR-MISMATCH-4
Both NS are in-bailiwick and exist with correct glue in the delegation, but there
are no address records in the zone matching the glue records.

* Zone: ib-addr-mismatch-4.consistency05.xa
  * Neither ns1 nor ns2 are defined in the zone as address records.
  * The correct NS records are in the zone.

### EXTRA-ADDRESS-CHILD
Child zone has one extra address record on the NS name.

* Zone: extra-address-child.consistency05.xa
  * The zone has address records for ns2 that match glue, but in addition
    the zone has extra A and AAAA records for ns2.
  * Both ns2 servers (both sets of IP addresses from child) must give identical
    DNS responses.

### OOB-ADDR-MISMATCH
For one NS (out-of-bailiwick, but with glue) glue does not match AA address
response.

* Zone: child.oob-addr-mismatch.consistency05.xa
  * Both ns1 and ns2 are out-of-bailiwick
  * ns1 is "ns1.sibbling.oob-addr-mismatch.consistency05.xa"
  * ns2 is "ns2.sibbling.oob-addr-mismatch.consistency05.xa"
  * Delegation is with glue.
  * The test zone ("child") has no address records for the NS names.
  * The "sibling" zone has full set of address records
  * ns1 in the "sibling" zone matches the addresses of glue.
  * ns2 in the "sibling" zone does not match the addresses of glue.
  * All IP addresses of ns1 and ns2 must serve identical versions of the zone.


[CONSISTENCY05]:                                                  ../../tests/Consistency-TP/consistency05.md
[RCODE Name]:                                                     https://www.iana.org/assignments/dns-parameters/dns-parameters.xhtml#dns-parameters-6
[Test zone README file]:                                          ../README.md
[Zone setup for test scenarios]:                                  #zone-setup-for-test-scenarios

