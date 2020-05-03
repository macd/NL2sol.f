# NL2sol.f: Non-linear least squares optimization

NL2SOL solves the non-linear least squares problem.  That is, it finds
an x that minimizes $ \sum_{i=1}^{n} {{r}_{i}}^{2}(x) $ where x is a
vector of size p. This is a copy of the original Fortran 77 NL2SOL 2.2
source code from netlib

The code and theory behind it are discussed in the following two
papers from Transactions on Mathematical Software (TOMS).

J.E. Dennis, D.M. Gay, R.E. Welsch, "An Adaptive Nonlinear
Least-Squares Algorithm", ACM Transactions on Mathematical Software
(TOMS), Volume 7 Issue 3, Sept. 1981, pp 348-368, ACM New York, NY, USA
[see here](http://dl.acm.org/citation.cfm?id=355965&CFID=660003329&CFTOKEN=25049918)

J.E. Dennis, D.M. Gay, R.E. Welsch, "Algorithm 573: NL2SOL—An Adaptive
Nonlinear Least-Squares Algorithm", ACM Transactions on Mathematical
Software (TOMS), Volume 7 Issue 3, Sept. 1981, pp 369-383, ACM New
York, NY, USA [see here](http://dl.acm.org/citation.cfm?id=355966)

Here we use the original NL2SOL Fortran 77 source code which appears
as TOMS ALgorithm 573 (NL2SOL Version 2.2).  The code was downloaded
from netlib and is archived in the **nl2sol** directory as a
single blob in the file named nl2sol.netlib.orig.f

This blob has also been broken up into the individual source files and
commented out the "c/6" code for the "c/7" code, which enables the f77
version.  Also added are cmake files for building the code and running
the tests.  Running the Fortran tests and coverage is manual and
not part of the installation. (The coverage is a very respectable 87%)
The original fortran test code now lives in a separate subdirectory
**tests** as well.  To learn how to build and run the tests
with coverage, see CMakeLists.txt

