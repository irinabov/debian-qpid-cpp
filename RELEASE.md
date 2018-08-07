### Building a release for vote:

1. Grab a clean checkout for safety.
2. Run: "git checkout ${BRANCH}" if needed to switch to branch of the intended release point.
3. Update the versions etc:
  - VERSION.txt
  - management/python/setup.py
  - docs/man/qpidd.1 (and the date)
  - src/amqp.cmake (ensure current proton release is marked tested)
4. Commit the changes, tag them.
  - Run: "git add ."
  - Run: 'git commit -m "update versions for ${TAG}"'
  - Run: 'git tag -m "tag ${TAG}" ${TAG}'
5. Run: "bin/export.sh $PWD ${TAG}" to create the qpid-cpp-${VERSION}.tar.gz release archive.
6. Create signature and checksum files for the archive:
  - e.g "gpg --detach-sign --armor qpid-cpp-${VERSION}.tar.gz"
  - e.g "sha512sum qpid-cpp-${VERSION}.tar.gz > qpid-cpp-${VERSION}.tar.gz.sha512"
7. Push branch changes and tag.
  - Also update versions to the applicable snapshot version for future work on it.
8. Commit artifacts to dist dev repo in https://dist.apache.org/repos/dist/dev/qpid/cpp/${TAG} dir.
9. Send vote email, provide links to dist dev repo and JIRA release notes.

### After a vote succeeds:

1. If needed, tag the RC bits with the final name/version.
2. Commit the artifacts to dist release repo in https://dist.apache.org/repos/dist/release/qpid/cpp/${VERSION} dir:
   - e.g: svn cp -m "add files for qpid-cpp-${VERSION}" https://dist.apache.org/repos/dist/dev/qpid/cpp/${TAG}/ https://dist.apache.org/repos/dist/release/qpid/cpp/${VERSION}/
3. Give the mirrors some time to distribute things. Usually 24hrs to be safe, less if needed.
   - https://www.apache.org/mirrors/ gives stats on mirror age + last check etc.
4. Update the website with release content.
5. Send release announcement email.
