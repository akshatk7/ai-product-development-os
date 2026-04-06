# [Project Name] — Launch Checklist

Standard gates for shipping. Check off items as they're completed.

---

## Pre-Launch

- [ ] Feature flag configured and tested in staging
- [ ] E2E tests passing
- [ ] Load testing complete (if applicable)
- [ ] Rollback plan documented
- [ ] On-call team briefed
- [ ] Monitoring dashboards set up
- [ ] Analytics instrumented (see `analytics-spec.md`)
- [ ] Error handling and edge cases verified

## Ship Review

- [ ] PM sign-off
- [ ] Eng sign-off
- [ ] Design sign-off
- [ ] Legal / compliance review (if applicable)
- [ ] Security review (if applicable)

## Launch

- [ ] Feature flag enabled for initial % rollout
- [ ] Monitoring during rollout (first 1h)
- [ ] Customer support / sales team briefed (if user-facing)
- [ ] Internal announcement posted

## Post-Launch

- [ ] Metrics check at 1 hour
- [ ] Metrics check at 1 day
- [ ] Metrics check at 1 week
- [ ] Experiment readout scheduled (if A/B test)
- [ ] Decision: ramp to 100% / iterate / roll back
- [ ] Update project README.md status to "Live"
