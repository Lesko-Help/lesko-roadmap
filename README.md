# lesko-roadmap

The new-member roadmap for the Lesko Help community.

**`index.html` is still the page**, and this repo is still where it is edited. Edit it, push to
`main`, and the change is live within about five minutes.

## Where it is actually served from

Not Vercel any more. The page is now served by a small Cloud Run service
(`lesko-roadmap`, project `lesko-486515`, region `europe-west1`), which **fetches this
`index.html` and re-serves it**. That service adds the two things a static page cannot do:

* signs the member in through Mighty Networks, and
* remembers which steps each member has completed, in BigQuery,

so a member's progress follows them between devices instead of living in one browser, and the
community can see where people get stuck.

It also ticks steps a member has already completed elsewhere — turning up to a class, posting in the
Questions channel — so the roadmap reflects what they have actually done, not only what they
remembered to click.

`vercel.json` redirects the old Vercel URL to the live one, so any link already posted keeps working.

## Editing the page

Nothing to install, nothing to build. Edit `index.html`, push, done.

Two things to be careful of, because they are load-bearing for the tracking:

* **Do not change a step's `data-id`.** That string is the key the warehouse joins on. Renaming one
  detaches every member's saved progress for that step. The ids are deliberately out of order
  (`step-2` is Day 1) so that the days can be reordered without touching them.
* **Adding or removing a step** needs a matching row in `ref_roadmap_steps` in the warehouse,
  or it will show on the page but count towards nothing.
