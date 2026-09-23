# A/B test significance analysis

Statistical significance for four conversion metrics across four A/B tests,
with segment breakdowns and correction for multiple comparisons.

**[Dashboard](link)**
TBD

## Method
1.  **Overall, per test.** Each metric is compared between test groups with a two-proportion z-test (α = 0.05, two-sided). No correction is applied at this level. The four metrics are funnel stages of the same test

2. **Segments.** The same calculation per test, broken down by segment. A segment is tested only if each group has at least 10 conversions and 10 non-conversions – the success-failure condition for the z-test. Below that, p-values are unreliable, so the segment is marked `is_testable = False` and gets no verdict.

3. **BH correction.** For multiple tests comparison to limit false positives, segment p-values are corrected with Benjamini-Hochberg (FDR). A family is one test × one breakdown dimension (segment). Each family is corrected separately; `n_family` records how many comparisons are in the family. Untestable segments are excluded before correction.

4. **Confidence intervals.** 95% confidence intervals on the absolute difference in conversion rates, uncorrected. The interval shows how precisely the conversion difference was measured in that segment.

## Result
TBD

