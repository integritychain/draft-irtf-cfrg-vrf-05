# Verifiable Random Functions

A self-contained Python 3 reference implementation of [draft-irtf-cfrg-vrf-05](https://tools.ietf.org/pdf/draft-irtf-cfrg-vrf-05.pdf)
corresponding to the ECVRF-EDWARDS25519-SHA512-Elligator2 cipher suite configuration,
suitable for  testing, porting and the generation of test vectors. It is inefficient
and not fully secure (e.g. not side-channel resistant, no memory scrubbing etc) and
thus should not be used in production. 

Significant portions of the lower-level ed25519-related code was adapted from that 
provided in Appendix A of [RFC 8032](https://tools.ietf.org/pdf/rfc8032.pdf). 

The `ecvrf_edwards25519_sha512_elligator2.py` file retains a significant amount of
documentation extracted from the specification, and provides a simple API as follows:

```python
# Section 5.1. ECVRF Proving
def ecvrf_prove(SK, alpha_string, test_dict=None):
    """
    Input:
        SK - VRF private key
        alpha_string - input alpha, an octet string
        test_dict - optional dict of samples to assert and/or record
    Output:
        pi_string - VRF proof, octet string of length ptLen+n+qLen
        If a test_dict is supplied, one will be returned
    """
    ...


# Section 5.2. ECVRF Proof To Hash
def ecvrf_proof_to_hash(pi_string, test_dict=None):
    """
    Input:
        pi_string - VRF proof, octet string of length ptLen+n+qLen
        test_dict - optional dict of samples to assert and/or record
    Output:
        "INVALID", or beta_string - VRF hash output, octet string of length hLen
        If a test_dict is supplied, one will be returned
    Important note:
        ECVRF_proof_to_hash should be run only on pi_string that is known to have been
        produced by ECVRF_prove, or from within ECVRF_verify as specified in Section 5.3.
    """
    ...


# Section 5.3. ECVRF Verifying
def ecvrf_verify(Y, pi_string, alpha_string, test_dict=None):
    """
    Input:
        Y - public key, an EC point
        pi_string - VRF proof, octet string of length ptLen+n+qLen
        alpha_string - VRF input, octet string
        test_dict - optional dict of samples to assert and/or record
    Output:
        ("VALID", beta_string), where beta_string is the VRF hash output, octet string
        of length hLen; or "INVALID"
        If a test_dict is supplied, one will be returned
    """
```

`ecvrf_edwards25519_sha512_elligator2_test.py` applies test cases drawn directly from
the specification.

## License

Copyright (C) 2020 Eric Schorn <eschorn@integritychain.com>

This program is free software; you can redistribute it and/or modify it under the terms
of the GNU General Public License version 3 as published by the Free Software Foundation

This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY;
without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
See the GNU General Public License for more details.

See the `LICENSE` file for additional information.