# A/B test significance analysis

Statistical significance for four conversion metrics across four A/B tests,
with segment breakdowns and correction for multiple comparisons.

**[Tableau Dashboard]([link](https://public.tableau.com/views/ABTestSignificance_17902127305650/ABSignificanceAnalysis?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link))**
TBD

## Method
1.  **Overall, per test.** Each metric is compared between test groups with a two-proportion z-test (α = 0.05, two-sided). No correction is applied at this level. The four metrics are funnel stages of the same test

2. **Segments.** The same calculation per test, broken down by segment. A segment is tested only if each group has at least 10 conversions and 10 non-conversions – the success-failure condition for the z-test. Below that, p-values are unreliable, so the segment is marked `is_testable = False` and gets no verdict.

3. **BH correction.** For multiple tests comparison to limit false positives, segment p-values are corrected with Benjamini-Hochberg (FDR). A family is one test × one breakdown dimension (segment). Each family is corrected separately; `n_family` records how many comparisons are in the family. Untestable segments are excluded before correction.

4. **Confidence intervals.** 95% confidence intervals on the absolute difference in conversion rates, uncorrected. The interval shows how precisely the conversion difference was measured in that segment.

## Results

### Test 1
**Overall:** positive. add_payment_info +12.5%, add_shipping_info +6.6%, begin_checkout +6.7%, all significant. new_accounts -3.4%, not significant.

**Where it worked:**
- Desktop and mobile both positive (+11% to +17% on add_payment_info)
- Canada (7.4% of traffic): +45%, +35%, +27% across three metrics
- India (9.5% of traffic): begin_checkout +27%
- Direct channel: add_payment_info +36%
- Small European markets (5.8% of traffic combined): +65% to +195% across metrics

**Where it didn't:**
- US (44% of traffic): new_accounts -9.7% — the only negative in the largest market, worth investigating
- Organic Search (35% of traffic): add_payment_info -19.5%
- Tablet (2.2% of traffic): begin_checkout -33%, too small

**Recommendation:** roll out. Investigate the US new_accounts drop and the Organic Search decline before scaling spend. Categorise the Undefined channel (7.4% of traffic).

### Test 2
**Overall:** no effect. All four metrics moved slightly up (+1.2% to +3.6%), none significant (p = 0.22 to 0.56).

**Where it worked:**
- US (44% of traffic): add_payment_info +20.2% 
- Undefined channel (7.3% of traffic): +25% to +31% across three metrics
- Australia, Mexico, Turkey (~1% each of traffic): begin_checkout +94% to +132%

**Where it didn't:**
- Canada (7.4% of traffic): -25% to -29% across three metrics
- Organic Search (35% of traffic): begin_checkout -9.5%, add_payment_info -14.3%
- UK (3.2% of traffic): add_payment_info -43.7%

**Recommendation:** do not roll out. Investigate why Canada reacted negatively and retest.

### Test 3
**Overall:** negative. begin_checkout -3.4% (p = 0.012). The other three metrics moved little and were not significant.

**Where it worked:**
- Undefined channel (6.5% of traffic): begin_checkout +12.7%
- Belgium, Colombia (<1% of traffic each): begin_checkout +97% to +148%

**Where it didn't:**
- US (44% of traffic): begin_checkout -10.2%, add_shipping_info -8.3%
- Organic Search (35% of traffic): begin_checkout -8.0%
- Taiwan (1.7% of traffic), Switzerland (0.5% of traffic), Croatia, Kuwait: begin_checkout -28% to -58%

**Recommendation:** do not roll out.

### Test 4
**Overall:** negative. begin_checkout -2.4% (p = 0.046), new_accounts -3.4% (p = 0.018). add_payment_info -3.5% and add_shipping_info -3.4% trend negative but are not significant (p = 0.12 and 0.07).

**Where it worked:**
- Mobile (39% of traffic): begin_checkout +4.3%
- Paid Search (28% of traffic): begin_checkout +6.9%
- India (9.4% of traffic): begin_checkout +16.9%
- Mexico, Netherlands, Indonesia (~1% of traffic each): begin_checkout +60% to +142%

**Where it didn't:**
- Desktop (58% of traffic): all three metrics negative, begin_checkout -5.7%
- Organic Search (36% of traffic): begin_checkout -8.0%
- Tablet (2.3% of traffic): -25% to -49% across three metrics
- Taiwan (1.7% of traffic), UK (3.2% of traffic), France (2.0% of traffic), China (1.8% of traffic): begin_checkout -21% to -51%

**Recommendation:** do not roll out. Desktop (58% of traffic) was affected the most. The entry to checkout: begin_checkout fell 5.7% (p = 0.0002), while conversion between the later funnel steps was stable. Investigate what the change did to the begin_checkout on desktop, fix it, and retest.



