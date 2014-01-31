# FAQ - EOL #

## Official FAQ ##

This is the official FAQ. For additional FAQs please visit our [GitHub repository](https://github.com/vrtadmin/clamav-faq) . You are encouraged to contribute to them.

## End of Life Policy (EOL) ##

The naming convention for ClamAV releases uses three numbers (X.Y.Z) where the first two (X.Y) identify a major release and the last one (Z) a minor release.

As of January 14 2014, the latest major release is 0.98 and the latest minor release is 0.98.1.

Before releasing a CVD update, we verify that it can be correctly loaded by the latest two major releases of ClamAV and all the minor versions released after each of them.

We only check the CVD update for false positives using the latest minor release (or major release, in case there has been no minor release after the last major release).
We only release security fixes for the latest minor release (or major release, in case there has been no minor release after the last major release).

**Disclaimer**: if this policy has to change due to a compatibility problem that prohibits the use of new detection technology, or impacts the stability of ClamAV infrastructure, we will announce the end of life for those versions four months before they become unsupported.
