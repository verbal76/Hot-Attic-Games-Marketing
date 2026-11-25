# Upwardly — Google Play Submission Automation Handoff
**Checkpoint:** 2026-11-24, approximately 21:34 EST  
**Status:** PAUSED BY OWNER — configuration only; no further actions authorized.

## Critical owner instruction
**Build 87 V1 has already been manually submitted to Google Play and is in review. DO NOT dispatch the Google Play Submission workflow, build or upload another AAB, create a draft, submit another release, or alter the existing Play submission.** This checkpoint documents setup for **future** releases only. Any live workflow execution, including `verify`, requires separate explicit owner authorization.

## Where we stopped
We were about to open the GitHub Actions **Variables** settings for `verbal76/Climb-Up` and create **two repository variables**. They have **NOT** been entered yet, based on the conversation. Google Cloud configuration through service-account IAM binding **is complete** according to successful Cloud Shell output.

**Resume at:** https://github.com/verbal76/Climb-Up/settings/variables/actions

Add under **Repository variables** (not secrets), if not already present:

| Variable | Exact value |
| --- | --- |
| `GOOGLE_PLAY_WIF_PROVIDER` | `projects/327670547936/locations/global/workloadIdentityPools/github/providers/climb-up` |
| `GOOGLE_PLAY_WIF_SERVICE_ACCOUNT` | `upwardly-play-publisher@upwardly-play-publisher.iam.gserviceaccount.com` |

After saving, inspect GitHub environment settings and configure Play Console service-account permissions, **without dispatching any workflow**. Verify the names/values against the workflow documentation before proceeding.

## Google Cloud — COMPLETED and confirmed by screenshots
- Cloud project name: **Upwardly Play Publisher**
- Project ID: `upwardly-play-publisher`
- Project number: `327670547936`
- Enabled successfully: `androidpublisher.googleapis.com`, `iamcredentials.googleapis.com`, `sts.googleapis.com`, `iam.googleapis.com`
- Created service account: `upwardly-play-publisher@upwardly-play-publisher.iam.gserviceaccount.com`
- Created Workload Identity Pool: `github` (global)
- Created OIDC provider: `climb-up` (global), issuer `https://token.actions.githubusercontent.com`
- Provider mapped: `google.subject`, `attribute.repository_id`, `attribute.repository_owner_id`, `attribute.workflow_ref`, `attribute.environment`
- Provider admission condition restricts repository ID `1409752751`, owner ID `278774829`, exact workflow `verbal76/Climb-Up/.github/workflows/google-play-submission.yml@refs/heads/main`, branch `refs/heads/main`, event `workflow_dispatch`, and environments `google-play-checks` or `google-play-production`.
- Added service-account IAM binding `roles/iam.workloadIdentityUser` to principal set `principalSet://iam.googleapis.com/projects/327670547936/locations/global/workloadIdentityPools/github/attribute.repository_id/1409752751`.
- Cloud Shell confirmed: `Operation ... finished successfully`, `Created service account`, `Created workload identity pool [github]`, `Created workload identity pool provider [climb-up]`, and `Updated IAM policy for serviceAccount`.
- No service-account JSON key was created.

**Security note:** The marketing repository is public. This checkpoint includes non-secret IDs, email address and resource paths only. Do **not** commit passwords, OAuth tokens, signing keys, private credentials, or downloaded key files. Verify provider conditions and IAM bindings before any eventual live authentication.

## Upwardly pipeline — status reported by Claude
- Repository: https://github.com/verbal76/Climb-Up
- Permanent workflow: `.github/workflows/google-play-submission.yml` on `main`, pipeline-only commit `cb3a8c6f`.
- Workflow: https://github.com/verbal76/Climb-Up/actions/workflows/google-play-submission.yml
- Workflow ID: `429783442`.
- Draft review PR #43: https://github.com/verbal76/Climb-Up/pull/43 ; development branch `ci/google-play-submission`, reported SHA `eba50791`; not merged.
- Manually dispatched from **main** only. The actual game source is an exact SHA selected from a release branch matching `build-<N>/v<M>` or `build-<N>/final`; branch name alone does not authorize release.
- Modes: `verify` (read-only), `draft`, `submit` (selected rollout). Draft/submit require typed confirmation and production approval checkpoint where supported.
- Requires matching SHA/branch and successful Android/core test evidence, signing and emulator checks, valid AAB/upload key, and monotonically increasing Play versionCode. Refuses conflicting drafts or unfinished staged rollouts.
- Reported offline validation: 26 pipeline tests plus 24 reused upload tests passed; `actionlint` clean. **Live GitHub runner execution beyond the guard, Google sign-in, Google Play API, and production environment approval have not been verified.**
- Previously accepted guard-only dispatch from `main` skipped all jobs without a runner or Google call; **do not repeat it now**, as the permanent pipeline may execute.

## Existing Play release — DO NOT TOUCH
- Upwardly package: `com.hotatticgames.climbup`.
- Build 87 V1, versionCode `87`, versionName `1.3.0`; source commit `628f8de5`.
- Build evidence: https://github.com/verbal76/Climb-Up/actions/runs/45521347552
- Android run passed; AAB produced as `Build.87.V1.Full.Google.Play.Bundle.aab`.
- Owner manually submitted release; screenshot of Google Play Console showed **“Changes in review”**, and publishing submissions **“1 in review.”** No new submission required or permitted.
- No assertion here that Google has approved or published the release.

## Remaining one-time configuration — NOT CONFIRMED COMPLETE
1. **GitHub:** Create the two repository variables above. Check `google-play-checks` and `google-play-production` environments and workflow-required protection rules, especially reviewer availability and branch restrictions. Do not weaken safeguards silently.
2. **Google Play Console:** Invite/add the service account under **Users and permissions**, restricted to the Upwardly app. Claude's setup instructions called for view app information, testing-track release/manage permissions, and production release permissions. Review minimum permissions and actual UI before granting.
3. **Google Play signing:** Confirm upload-key certificate SHA-256 against the expected value `9C:2F:B5:BB:E0:E1:7E:D1:D2:1D:36:1F:15:7C:12:61:B9:30:03:16:39:42:3E:39:31:73:9D:EA:C1:03:B1:AE`.
4. **Workflow review:** Confirm its current `main` copy and docs agree with WIF provider conditions, environment names, and variables; verify GitHub required-reviewer support on the account.
5. **Future validation:** Only when owner separately authorizes, perform a controlled `verify` dispatch; no production upload or submission without further explicit authorization. Since versionCode 87 is already submitted, do not attempt to resubmit it.

## Resume protocol
1. Read this checkpoint and the canonical setup guide `docs/releases/GOOGLE_PLAY_SUBMISSION.md` in `verbal76/Climb-Up` on `main`.
2. Inspect current Google Play status without modifying the release.
3. Start with GitHub repository **Actions variables**; verify whether both variables already exist before adding them.
4. Work **one screen/step at a time** with the owner; preserve screenshots or success/error output.
5. **Do not trigger any workflow, build, upload, release, submit, merge, or create scheduled checks** unless separately authorized.

**Last completed action:** Google Cloud Shell IAM policy binding for `roles/iam.workloadIdentityUser` succeeded. **Next action:** add the two GitHub repository variables.
