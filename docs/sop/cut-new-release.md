These instructions assume we are moving from release X.A to X.B and that origin#master contains X.A's deisred content.

- Have RCM create a new "Release" in the Errata Tool so that Advisories can be created for it.

- Have RCM start creating new tags for X.B and new dist-git branches for X.B

- Have RCM create product listings for X.B from X.A. Product Listings are apparently "just a for loop" on RCM's end. It might be possible to get this automated. Though there might be bookkeeping steps as well.

- origin will create a new branch release-X.A and begin including changes for X.B in origin#master

- Create a new branch ose#enterprise-X.A from ose#master

- Create a new branch openshift-ansible#release-X.A from openshift-ansible#master

- Create a new branch in openshift/jenkins: openshift-X.B

- Remind jcantril that release cut is occuring so they can make branch for origin-aggregated-logging.

- In ose#master, set origin.spec "Version: X.B.0" and "Release: "0.0.0%{?dist}"

- In openshift-anisble#master spec file specify version X.B.0 and Release "0.0.0%{?dist}"

- In ose#enterprise-X.A, make sure the release is to to "1%{?dist}" (unless it has already been changed to a non 0. value)

- Create a new origin-web-console#enterprise-X.B from origin-web-console#master. At this point, the origin-web-console team will start merging X.A changes into origin-web-console#X.A  and changes for X.B into origin-web-console#master . They will also add a branding commit to enterprise-X.B after few days. The branding commit is done manually and not CD's responsibility.

- Create a new origin-web-console-server#enterprise-X.B branch from origin-web-console-server#master.

- Add a X.B .tito/releasers.conf to ose#master and openshift-ansible#master

- In aos-cd-jobs:build-scripts/puddle-conf, create new puddle configuration files for X.B:
  - atomic_openshift-X.B.conf   (you can copy the content of the X.A file, but several changes are required inside)
  - errata-puddle-X.B.conf
  - errata-puddle-X.B-signed.conf

- In aos-cd-jobs
  - Change build/ocp Jenkinsfile:
    - Add X.B as a choice in the build options
  - Change build/ose Jenkinsfile:
    - OSE_MASTER=X.B

- Copy dist-git data from last version branches into new version branches using doozer images:merge-branch

- In enterprise-images github repo, follow the instructions in NewBranchProcess.md
- Create a new build/ose-X.B job to enable single click invocation of a build/ocp for the version

# Before doing a deployment
- Vendor the new openshift-ansible build into openshift-tools: https://github.com/openshift/ops-sop/blob/369828ae043abbb5031dabfbcde3c8ae3fdbbfa2/v3/vendoringopenshiftinstaller.asciidoc#getting-the-correct-rpms-for-online-beta-vendoring
- Create X.B tech profiles (e.g. https://github.com/openshift/openshift-ansible-ops/pull/3584)
