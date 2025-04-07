[Master Branch] 
   │
   ├──1. Create Feature Branch
   │     (git checkout -b feature/metrics-tab)
   │
   ├──2. Implement Feature Toggles
   │     │
   │     ├── a. Add toggle configuration
   │     └── b. Wrap feature code in toggle check
   │
   ├──3. Regularly Sync with Master
   │     (git merge master → feature/metrics-tab)
   │
   ├──4. Make Minimal Changes
   │     (Only metrics tab related code)
   │
   ├──5. Create Pull Request
   │     (PR: feature/metrics-tab → master)
   │     │
   │     ├──6. PR Approvals
   │     │     (Code review + approvals)
   │     │
   │     └──7. QA Sign-off
   │           (Test in ephemeral env)
   │
   ├──8. Merge to Master
   │     (git merge --no-ff feature/metrics-tab)
   │     │
   │     ├──9. UAT Sign-off
   │     │     (Test in staging)
   │     │
   │     └──10. Deploy to Production
   │           (With toggle initially OFF)
   │           │
   │           ├──11. Monitor Deployment
   │           │     │
   │           │     ├── Success: Enable toggle
   │           │     └── Failure: Rollback/Disable
   │           │
   │           └──12. Tag Production Commit
   │                 (git tag prod-vX.Y.Z)
   │
   └──[Optional] Delete Feature Branch
         (git branch -d feature/metrics-tab)




         +------------------+------------+-------------+---------------+----------------+---------------+
|     Developer    |   QA Team  | Code Review |    DevOps     |  Prod Deploy   |    Monitor    |
+------------------+------------+-------------+---------------+----------------+---------------+
| 1. Create        |            |             |               |                |               |
| feature branch   |            |             |               |                |               |
| (feature/metrics)|            |             |               |                |               |
|------------------|------------|-------------|---------------|----------------|---------------|
| 2. Implement     |            |             |               |                |               |
| Feature Toggles: |            |             |               |                |               |
|  - Add config    |            |             |               |                |               |
|  - Wrap UI code  |            |             |               |                |               |
|------------------|------------|-------------|---------------|----------------|---------------|
| 3. Sync with     |            |             |               |                |               |
| master regularly |            |             |               |                |               |
|------------------|------------|-------------|---------------|----------------|---------------|
| 4. Minimal       |            |             |               |                |               |
| changes only     |            |             |               |                |               |
|------------------|------------|-------------|---------------|----------------|---------------|
| 5. Create PR     |            |             |               |                |               |
| against master   |            |             |               |                |               |
|------------------|------------|-------------|---------------|----------------|---------------|
|                  | 7. Test in | 6. Review   |               |                |               |
|                  | ephemeral  | PR & Approve|               |                |               |
|                  | QA env     |             |               |                |               |
|------------------|------------|-------------|---------------|----------------|---------------|
| 8. Merge to      |            |             |               |                |               |
| master after     |            |             |               |                |               |
| approvals        |            |             |               |                |               |
|------------------|------------|-------------|---------------|----------------|---------------|
|                  | 9. UAT     |             | 10. Deploy    |                |               |
|                  | Sign-off   |             | to Prod       |                |               |
|                  | (Staging)  |             | (Toggle OFF)  |                |               |
|------------------|------------|-------------|---------------|----------------|---------------|
|                  |            |             |               | 11. Monitor &  |               |
|                  |            |             |               | Enable Toggle  |               |
|------------------|------------|-------------|---------------|----------------|---------------|
|                  |            |             | 12. Tag Prod  |                |               |
|                  |            |             | Commit        |                |               |
+------------------+------------+-------------+---------------+----------------+---------------+


Ephemeral QA → Staging → Production (Toggle OFF → Monitor → Toggle ON)


Key Notes:

Feature toggle remains OFF during initial deployment

All testing happens before merge to master (shift-left testing)

Rollback is achieved by disabling the toggle rather than code reversal

Ephemeral environments are created on-demand for QA testing


====================================================================
+------------------+------------+-------------+---------------+----------------+---------------+
|     Developer    |   QA Team  | Code Review |    DevOps     |  Prod Deploy   |    Monitor    |
+------------------+------------+-------------+---------------+----------------+---------------+
| 1. Follow Case 1 |            |             |               |                |               |
| steps 1-8 (PR,   |            |             |               |                |               |
| merge to master) |            |             |               |                |               |
|------------------|------------|-------------|---------------|----------------|---------------|
| 2. Create        |            |             |               |                |               |
| bugfix branch    |            |             |               |                |               |
| from last prod   |            |             |               |                |               |
| tag (git checkout|            |             |               |                |               |
| -b bugfix/xyz    |            |             |               |                |               |
| prod-vX.Y.Z)     |            |             |               |                |               |
|------------------|------------|-------------|---------------|----------------|---------------|
| 3. Cherry-pick   |            |             |               |                |               |
| hotfix commit    |            |             |               |                |               |
| (git cherry-pick |            |             |               |                |               |
| abc123)          |            |             |               |                |               |
|------------------|------------|-------------|---------------|----------------|---------------|
|                  | 4. Test in |             |               |                |               |
|                  | lower envs |             |               |                |               |
|                  | (UI+Backend|             |               |                |               |
|                  | integration|             |               |                |               |
|                  | tests)     |             |               |                |               |
|------------------|------------|-------------|---------------|----------------|---------------|
|                  | 5. UAT     |             |               |                |               |
|                  | Sign-off   |             |               |                |               |
|                  | (Staging)  |             |               |                |               |
|------------------|------------|-------------|---------------|----------------|---------------|
|                  |            |             | 6. Deploy to  |                |               |
|                  |            |             | Production    |                |               |
|------------------|------------|-------------|---------------|----------------|---------------|
|                  |            |             |               | 7. Monitor &   |               |
|                  |            |             |               | Rollback Plan  |               |
|------------------|------------|-------------|---------------|----------------|---------------|
|                  |            |             | 8. Tag Prod   |                |               |
|                  |            |             | Commit        |                |               |
|                  |            |             | (git tag      |                |               |
|                  |            |             | prod-vX.Y.Z+1)|                |               |
+------------------+------------+-------------+---------------+----------------+---------------+



Key Differences from Case 1:
Branching Strategy

Bugfix branches are created from the last prod tag (not master) to avoid untested changes.

Cherry-Picking

Critical fix is selectively applied to the stable branch.

Integration Testing Emphasis

Explicit QA phase for UI+Backend integration in lower environments.

Rollback Preparedness

Higher risk due to untested changes → stronger monitoring/rollback emphasis.



[Master Branch] (Contains bug)
   │
   ├── [prod-vX.Y.Z Tag] (Stable)
   │     │
   │     └──2. Create bugfix branch
   │         (git checkout -b bugfix/xyz prod-vX.Y.Z)
   │         │
   │         ├──3. Cherry-pick fix
   │         │   (git cherry-pick abc123)
   │         │
   │         ├──4. Test in lower envs
   │         │   (UI+Backend integration)
   │         │
   │         ├──5. UAT Sign-off
   │         │
   │         └──6. Deploy to Prod
   │             │
   │             ├──7. Monitor
   │             │   ├── Success: Keep
   │             │   └── Failure: Rollback
   │             │
   │             └──8. Tag (prod-vX.Y.Z+1)
   │
   └── [Optional] Merge fix back to master
         (git checkout master; git merge bugfix/xyz)


         Critical Notes:
Hotfix Isolation: The bugfix branch avoids master's untested changes.

Double Testing: Fix is validated in both integration env and staging.

Tag-Based Tracking: Prod releases are strictly tag-controlled.
