# Communication-aware Message Matching in MPI

This repo implements partner/non-partner message queue design in MVAPICH2. The details of the design is available at https://www.queensu.ca/academia/afsahi/pprl/papers/CCPE-2018-2.pdf

The design is mostly imlemented at src/mpid/ch3/src/ch3u_recvq.c

# MVAPICH2 QUICK START GUIDE

The MVAPICH2 software, based on MPI 3.1 standard, delivers the best performance,
scalability and fault tolerance for high-end computing systems and servers using
InfiniBand, Omni-Path, Ethernet/iWARP, and RoCE networking technologies.
MVAPICH2 provides underlying support for several interfaces (such as OFA-IB,
OFA-iWARP, OFA-RoCE, CH3-PSM, CH3-PSM2, Shared Memory, and TCP) for portability
across multiple networks.

This package uses the basic GNU build system.  To install a default build of
MVAPICH2 from the release tarball you can issue the following commands:

    ./configure --prefix=/path/to/install/mvapich2
    make            # make -j<num threads> for parallel build
    make install

The latest MVAPICH2 release tarball can be downloaded from:

    http://mvapich.cse.ohio-state.edu/download/mvapich2/

If you are downloading MVAPICH2 from svn, you can use the following steps to
install a default build.

    ./autogen.sh
    ./configure --prefix=/path/to/install/mvapich2
    make            # make -j<num threads> for parallel build
    make install

For more information about this package and the MVAPICH project please refer to
the project web site.  This site is frequently updated with latest releases,
publications, benchmarks, installation guides and performance tuning
guidelines.

    http://mvapich.cse.ohio-state.edu/overview/

It is highly suggested that you take a look at the MVAPICH2 userguide in order
to ensure that you're using the build options that are ideal for your
installation.  The latest documentation can be found at the following url:

    http://mvapich.cse.ohio-state.edu/support/

New users may also be interested in the expected MVAPICH2 performance numbers.
This can be found at the following link.

    http://mvapich.cse.ohio-state.edu/performance/

If you would like to get information about future updates, releases,
publications, etc. related to the MVAPICH2 project, you may subscribe to the
mvapich mailing list. This is an announcement-only list. All release
announcements related to MVAPICH2 are posted to this list.

    http://mail.cse.ohio-state.edu/mailman/listinfo/mvapich/

There is also an mvapich-discuss mailing list associated with this project. You
may subscribe to this list to participate in discussions related to MVAPICH2 as
well as submitting bug reports and suggestions.

    http://mail.cse.ohio-state.edu/mailman/listinfo/mvapich-discuss/
