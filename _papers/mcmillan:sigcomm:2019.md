---
ENTRYTYPE: inproceedings
abstract: QUIC is a new Internet secure transport protocol currently in the process of IETF standardization. It is intended as a replacement for the TLS/TCP
  stack and will be the basis of HTTP/3, the next official version of the hypertext transfer protocol. As a result, it is likely, in the near future, to
  carry a substantial fraction of traffic on the Internet. We describe our experience applying a methodology of compositional specification-based testing
  to QUIC. We develop a formal specification of the wire protocol, and use this specification to generate automated randomized testers for implementations
  of QUIC. The testers effectively take one role of the QUIC protocol, interacting with the other role to generate full protocol executions, and verifying
  that the implementations conform to the formal specification. This form of testing generates significantly more diverse stimuli and stronger correctness
  criteria than interoperability testing, the primary method used to date to validate QUIC and its implementations. As a result, numerous implementation
  errors have been found. These include some vulnerabilities at the protocol and implementation levels, such as an off-path denial of service scenario and
  an information leak similar to the "heartbleed" vulnerability in OpenSSL.
added: 2021-04-17
address: New York, NY, USA
authors:
- Kenneth L. McMillan
- Lenore D. Zuck
booktitle: Proceedings of the ACM Special Interest Group on Data Communication
doi: 10.1145/3341302.3342087
isbn: '9781450359566'
layout: paper
link: https://doi.org/10.1145/3341302.3342087
location: Beijing, China
numpages: '14'
pages: 227-240
publisher: Association for Computing Machinery
read: true
readings:
- 2021-04-17
series: SIGCOMM '19
title: Formal specification and testing of QUIC
url: https://doi.org/10.1145/3341302.3342087
year: 2019
notes:
- test-generation
- Ivy verifier
- symbolic execution
- weakest precondition
- QUIC protocol
papers:
- rath:epiq:2018
---

The [QUIC protocol] is a Google-designed protocol providing a secure, authenticated replacement for TLS/TCP (i.e., to replace https).
The goal of this paper is to improve on the use of interoperability testing to test the QUIC specification
and implementations.
They create a "substantial" specification of the QUIC standard in the [Ivy verifier] language
from which they generate tests to use with implementations
using a compositional approach (assume/guarantee reasoning) and constrained random generation.
They found bugs in implementations and a potential DoS in the protocol.
Some parts of the informally specified standard were hard to formalize effectively.
They focus on safety but discuss liveness.

Tests are generated by generating random sequences of events while accumulating
constraints on parameters in a way that can be seen as [symbolic execution] or
[weakest precondition] computation.

Their specification is mostly *extensional* (talks about externally visible behaviour)
whereas the QUIC standard is *intentional* (talks about internal implementation details).
*Intentional* specs can be a bit more tractable because they are less non-deterministic.
Their spec is partly *intentional* in that it talks about events occuring between layers
of the network stack.
They found some of the intentional parts of the spec were not effectively testable
--- although this does not always mean that the requirement is superfluous.

Tests events are generated at around 10Hz.
They found bugs at a rate of about one a day.
Triaging errors, refining the specs and working with implementors to
diagnose and fix errors was the time consuming thing.

An interesting limitation of this general approach is that tests are generated
in advance (i.e., offline). This prevented them from exploring adaptive behaviour
and, in particular, recovery and congestion control.
(To work online, they suggest running tests in virtual environments.)
They also don't (can't) explore liveness.
And, although they could potentially use their tests as fuzzing seeds to test
error detection, handling and robustness, they did not do so.

{% include links.html %}