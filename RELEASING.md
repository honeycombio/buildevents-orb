# Releasing

- Update [example](./src/examples/example.yml) with new version to be released.
- Update `CHANGELOG.md` with the changes since the last release.
  - Use below command to get a list of all commits since last release

    ```sh
    git log <last-release-tag>..HEAD --pretty='%Creset- %s | [%an](https://github.com/%an)'
    ```

  - Copy the output from the command above into the top of [changelog](./CHANGELOG.md)
  - fix each `https://github.com/<author-name>` to point to the correct github username
  (the `git log` command can't do this automatically)
  - organize each commit based on their prefix into below three categories:

    ```sh
    ### Features
      - <a-commit-with-feat-prefix>

    ### Fixes
      - <a-commit-with-fix-prefix>

    ### Maintenance
      - <a-commit-with-maintenance-prefix>
    ```

- Commit changes, push, and open a release preparation pull request for review.
- Once the pull request is merged, fetch the updated `main` branch.
- Apply a tag for the new version on the merged commit (e.g. `git tag -a v2.3.1 -m "v2.3.1"`)
- Push the new version tag up to the project repository to kick off build and artifact publishing to the Orb registry e.g. `git push origin v2.3.1`
- Create a release in GitHub from the new tag.
- Click "generate release notes" in GitHub for full changelog notes and any new contributors.
- Publish the GitHub release.
