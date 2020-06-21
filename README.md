Overview
========
This gwt-java-math library provides an efficient java.math implementation for GWT.

**This library has been merged into GWT 2.1 itself!**

For those who still want java.math for GWT 1.5 - 1.7 it will continue to be supported.

The library is based on Apache Harmony's implementation of java.math but has been modified to run efficiently in Java Script.

Features
--------

- An almost complete implementation of java.math.BigInteger, java.math.BigDecimal, java.math.MathContext, java.math.RoundingMode and java.util.Random (see caveat 1)
- Passes almost all Apache Harmony's unit tests in web mode. (see caveat 2)
- GWT serialization of BigInteger, BigDecimal and MathContext so they can be used in the GWT RPC mechanism.
- The library is written purely in Java and requires no external Java Script. This allows the GWT compiler to strip away all the math operations that your application does not require and produce the smallest and most efficient application.

Implementation Details
======================

Harmony's implementation of BigDecimal switches, depending on the precision of the number, between a long (if the precision is less than 64 bits) and a BigInteger (if the precision is greater than or equal to 64 bits), for the BigDecimal's unsigned value. This allows operations on numbers within the 64 bit precision to be done efficiently using long arithmetic rather than BigInteger arithmetic. As GWT emulates longs this efficiency gain is significantly negated.

To solve this, this gwt-java-math library modifies Harmony's implementation to switch between a double (if the precision is less than 54 bits) and a BigInteger (if the precision is greater than or equal to 54 bits), for the BigDecimal's unsigned value. 54 bits is the maximum precision a double supports. This allows operations on numbers within the 54 bit precision to be done efficiently using double arithmetic rather than BigInteger arithmetic.

Caveats
=======

- GWT does not support Double.doubleToLongBits(double value) which is required to correctly implement the BigDecimal(double) constructor. The static method BigDecimal.valueOf(double) is available and is more likely the behaviour you want.
- The unit tests that do not pass in web mode involve rounding behaviour of floats which is done using double precision in GWT.
- The implementation of java.util.Random is based around long arithmetic which is emulated in GWT and therefor not very efficient.

More Info
=========

- GettingStarted
- RoadMap
- ReleaseNotes

If you use this project let me know by sending me, the project owner, an email. If you have any general questions, thoughts, or ideas send them to the discussion group.

Checkout my other projects: gwt-comet.

