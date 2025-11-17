# Complete CSS Learning Roadmap - Obsidian Vault Ready
**Structured • Clickable • Dataview + Callouts**
> [!info] Obsidian Setup
> - **Paste** this entire file → `CSS Roadmap.md`
> - **Enable plugins**: `Dataview`, `Admonition`
> - Click any `[[Subtopic]]` → auto-creates note
> - Use **Graph View** to see connections
> - Add `status:: todo` or `status:: done` to track progress
---
## [[#How to Use This Roadmap|How to Use This Roadmap]]
> [!tip] Learning Path
> 1. **Phase 1 (Weeks 1–2)** → [[#Table 1: CSS Fundamentals]]
> 2. **Phase 2 (Week 3)** → [[#Table 2: Box Model & Layout Basics]]
> 3. **Phase 3 (Week 4)** → [[#Table 3: Advanced Selectors]]
> 4. **Phase 4 (Week 5)** → [[#Table 4: Colors & Backgrounds]]
> 5. **Phase 5 (Week 6)** → [[#Table 5: Typography]]
> 6. **Phase 6 (Week 7)** → [[#Table 6: Positioning]]
> 7. **Phase 7 (Weeks 8–9)** → [[#Table 7: Flexbox]]
> 8. **Phase 8 (Weeks 10–11)** → [[#Table 8: Grid Layout]]
> 9. **Phase 9 (Weeks 12–13)** → [[#Table 9: Responsive Design]]
> 10. **Phase 10 (Week 14)** → [[#Table 10: Transforms & Transitions]]
> 11. **Phase 11 (Weeks 15–16)** → [[#Table 11: Animations]]
> 12. **Phase 12 (Weeks 17–18)** → [[#Table 12: Advanced Layout Techniques]]
> 13. **Phase 13 (Weeks 19–20)** → [[#Table 13: Modern CSS Features]]
> 14. **Phase 14 (Weeks 21–22)** → [[#Table 14: CSS Architecture]]
> 15. **Phase 15 (Weeks 23–24)** → [[#Table 15: Performance & Optimization]]
> 16. **Phase 16 (Ongoing)** → [[#Table 16: Accessibility & Best Practices]]
> [!success] Pro Tip
> After every table: **Build a mini-project** → validate at [[validator.w3.org]]
---
## Dataview: Roadmap Overview
```dataview
TABLE
  level AS "Level",
  status AS "Status"
FROM #roadmap
SORT file.name ASC
```
---
## [[#Table 1: CSS Fundamentals|Table 1: CSS Fundamentals]] 🟢 Beginner

> `level:: beginner` | `tags:: #css #fundamentals` | `status:: todo`

| Topic | Clickable Subtopics |
|-------|---------------------|
| **CSS Basics** | [[What is CSS]], [[CSS syntax]], [[CSS rules]], [[Selectors]], [[Properties]], [[Values]], [[CSS comments]], [[Case sensitivity]], [[Adding CSS]], [[Inline styles]], [[Internal stylesheets]], [[External stylesheets]], [[Link element]], [[Import rule]], [[Multiple stylesheets]], [[Stylesheet organization]] |
| **Basic Selectors** | [[Universal selector]], [[Type selector]], [[Class selector]], [[ID selector]], [[Attribute selector]], [[Grouping selectors]], [[Selector specificity basics]] |
| **Color** | [[Color keywords]], [[Hexadecimal colors]], [[RGB colors]], [[RGBA colors]], [[HSL colors]], [[HSLA colors]], [[Currentcolor keyword]], [[Transparent keyword]], [[Color contrast]] |
| **Units** | [[Absolute units]], [[Pixels]], [[Points]], [[Inches, centimeters]], [[Relative units]], [[Percentages]], [[Em units]], [[Rem units]], [[Viewport units (vw, vh)]], [[Viewport min/max]], [[Ch unit]], [[Ex unit]], [[Zero values without units]] |
| **Text Styling** | [[Font family]], [[Font size]], [[Font weight]], [[Font style]], [[Font variant]], [[Line height]], [[Letter spacing]], [[Word spacing]], [[Text align]], [[Text decoration]], [[Text transform]], [[Text indent]], [[Text shadow]], [[White space]], [[Word break]], [[Word wrap]], [[Text overflow]], [[Vertical align]] |

---
## [[#Table 2: Box Model & Layout Basics|Table 2: Box Model & Layout Basics]] 🟢 Beginner

> `level:: beginner` | `tags:: #css #box-model` | `status:: todo`

| Topic | Clickable Subtopics |
|-------|---------------------|
| **Box Model** | [[Content box]], [[Padding]], [[Border]], [[Margin]], [[Box model visualization]], [[Width and height]], [[Box sizing property]], [[Content box sizing]], [[Border box sizing]] |
| **Padding** | [[Padding shorthand]], [[Padding top]], [[Padding right]], [[Padding bottom]], [[Padding left]], [[Padding with percentages]] |
| **Margin** | [[Margin shorthand]], [[Margin top]], [[Margin right]], [[Margin bottom]], [[Margin left]], [[Margin auto]], [[Margin collapse]], [[Negative margins]] |
| **Border** | [[Border width]], [[Border style]], [[Border color]], [[Border shorthand]], [[Border individual sides]], [[Border radius]], [[Border radius individual corners]], [[Border image]], [[Outline property]], [[Outline offset]] |
| **Display** | [[Block display]], [[Inline display]], [[Inline-block display]], [[None display]], [[Display vs visibility]], [[Visibility property]] |
| **Width and Height** | [[Width property]], [[Height property]], [[Max width]], [[Max height]], [[Min width]], [[Min height]], [[Aspect ratio]] |
| **Overflow** | [[Overflow visible]], [[Overflow hidden]], [[Overflow scroll]], [[Overflow auto]], [[Overflow x]], [[Overflow y]], [[Text overflow ellipsis]] |

---
## [[#Table 3: Advanced Selectors|Table 3: Advanced Selectors]] 🟡 Intermediate

> `level:: intermediate` | `tags:: #css #selectors` | `status:: todo`

| Topic | Clickable Subtopics |
|-------|---------------------|
| **Combinators** | [[Descendant combinator]], [[Child combinator]], [[Adjacent sibling combinator]], [[General sibling combinator]] |
| **Attribute Selectors** | [[Exact attribute match]], [[Attribute starts with]], [[Attribute ends with]], [[Attribute contains]], [[Attribute contains word]], [[Attribute with hyphen]], [[Case-insensitive matching]] |
| **Pseudo-classes** | [[Hover pseudo-class]], [[Active pseudo-class]], [[Focus pseudo-class]], [[Visited pseudo-class]], [[Link pseudo-class]], [[First child]], [[Last child]], [[Nth child]], [[Nth last child]], [[Nth of type]], [[Nth last of type]], [[First of type]], [[Last of type]], [[Only child]], [[Only of type]], [[Empty pseudo-class]], [[Not pseudo-class]], [[Target pseudo-class]], [[Enabled pseudo-class]], [[Disabled pseudo-class]], [[Checked pseudo-class]], [[Valid pseudo-class]], [[Invalid pseudo-class]], [[Required pseudo-class]], [[Optional pseudo-class]], [[Read-only pseudo-class]], [[Read-write pseudo-class]], [[In-range pseudo-class]], [[Out-of-range pseudo-class]], [[Focus-within pseudo-class]], [[Focus-visible pseudo-class]] |
| **Pseudo-elements** | [[Before pseudo-element]], [[After pseudo-element]], [[First-letter pseudo-element]], [[First-line pseudo-element]], [[Selection pseudo-element]], [[Placeholder pseudo-element]], [[Marker pseudo-element]], [[Backdrop pseudo-element]] |
| **Specificity** | [[Specificity calculation]], [[Inline style specificity]], [[ID specificity]], [[Class specificity]], [[Element specificity]], [[Specificity hierarchy]], [[Important declaration]], [[Specificity conflicts]] |

---
## [[#Table 4: Colors & Backgrounds|Table 4: Colors & Backgrounds]] 🟢→🟡

> `level:: beginner-intermediate` | `tags:: #css #colors #backgrounds` | `status:: todo`

| Topic | Clickable Subtopics |
|-------|---------------------|
| **Background Color** | [[Background color property]], [[Transparent backgrounds]], [[Opacity property]], [[RGBA for transparency]] |
| **Background Images** | [[Background image property]], [[Multiple background images]], [[Background repeat]], [[No repeat]], [[Repeat x]], [[Repeat y]], [[Background position]], [[Position keywords]], [[Position percentages]], [[Position pixels]], [[Background size]], [[Cover value]], [[Contain value]], [[Background attachment]], [[Fixed background]], [[Scroll background]], [[Background origin]], [[Background clip]], [[Background shorthand]] |
| **Gradients** | [[Linear gradients]], [[Gradient direction]], [[Gradient angle]], [[Color stops]], [[Multiple color stops]], [[Gradient transparency]], [[Radial gradients]], [[Radial shape]], [[Radial size]], [[Radial position]], [[Conic gradients]], [[Repeating linear gradients]], [[Repeating radial gradients]] |
| **Advanced Color** | [[Color interpolation]], [[Color spaces]], [[LAB colors]], [[LCH colors]], [[Color mixing]], [[Color functions]], [[Color contrast functions]] |

---
## [[#Table 5: Typography|Table 5: Typography]] 🟡 Intermediate

> `level:: intermediate` | `tags:: #css #typography` | `status:: todo`

| Topic | Clickable Subtopics |
|-------|---------------------|
| **Web Fonts** | [[Font face rule]], [[Font source]], [[Font format]], [[Font weight descriptor]], [[Font style descriptor]], [[Font display property]], [[Google Fonts]], [[Adobe Fonts]], [[Custom font hosting]], [[Font loading strategies]], [[Font subsetting]], [[Variable fonts]] |
| **Font Properties** | [[Font family stack]], [[Generic font families]], [[Serif fonts]], [[Sans-serif fonts]], [[Monospace fonts]], [[Cursive fonts]], [[Fantasy fonts]], [[System fonts]], [[Font size adjust]], [[Font stretch]], [[Font synthesis]] |
| **Text Properties** | [[Text align last]], [[Text align justify]], [[Text decoration line]], [[Text decoration color]], [[Text decoration style]], [[Text decoration thickness]], [[Text decoration skip]], [[Text underline offset]], [[Text emphasis]], [[Text orientation]], [[Text combine upright]] |
| **Advanced Typography** | [[Hyphenation]], [[Hanging punctuation]], [[Initial letter]], [[Leading trim]], [[Text spacing]], [[Font feature settings]], [[Font variant settings]], [[Ligatures]], [[Kerning]], [[OpenType features]], [[Writing mode]], [[Direction property]], [[Unicode bidi]] |

---
## [[#Table 6: Positioning|Table 6: Positioning]] 🟡 Intermediate

> `level:: intermediate` | `tags:: #css #positioning` | `status:: todo`

| Topic | Clickable Subtopics |
|-------|---------------------|
| **Position Property** | [[Static positioning]], [[Relative positioning]], [[Absolute positioning]], [[Fixed positioning]], [[Sticky positioning]], [[Position context]] |
| **Position Offsets** | [[Top offset]], [[Right offset]], [[Bottom offset]], [[Left offset]], [[Inset shorthand]], [[Position with percentages]], [[Position with viewport units]] |
| **Z-index** | [[Z-index property]], [[Stacking context]], [[Stacking order]], [[Z-index and positioning]], [[Negative z-index]], [[Z-index best practices]] |
| **Float** | [[Float left]], [[Float right]], [[Float none]], [[Clear property]], [[Clear both]], [[Clear left]], [[Clear right]], [[Clearfix technique]], [[Float collapse]] |

---
## [[#Table 7: Flexbox|Table 7: Flexbox]] 🟡 Intermediate

> `level:: intermediate` | `tags:: #css #flexbox` | `status:: todo`

| Topic | Clickable Subtopics |
|-------|---------------------|
| **Flex Container** | [[Display flex]], [[Flex direction row]], [[Flex direction column]], [[Flex direction reverse]], [[Flex wrap]], [[Flex wrap reverse]], [[Flex flow shorthand]], [[Justify content]], [[Justify content flex-start]], [[Justify content flex-end]], [[Justify content center]], [[Justify content space-between]], [[Justify content space-around]], [[Justify content space-evenly]], [[Align items]], [[Align items stretch]], [[Align items flex-start]], [[Align items flex-end]], [[Align items center]], [[Align items baseline]], [[Align content]], [[Align content distribution]], [[Gap property]], [[Row gap]], [[Column gap]] |
| **Flex Items** | [[Flex grow]], [[Flex shrink]], [[Flex basis]], [[Flex shorthand]], [[Align self]], [[Order property]], [[Flex item sizing]] |
| **Flexbox Patterns** | [[Holy grail layout]], [[Sticky footer]], [[Card layouts]], [[Navigation bars]], [[Equal height columns]], [[Centering content]], [[Space distribution]], [[Flexible forms]] |

---
## [[#Table 8: Grid Layout|Table 8: Grid Layout]] 🔴 Advanced

> `level:: advanced` | `tags:: #css #grid` | `status:: todo`

| Topic | Clickable Subtopics |
|-------|---------------------|
| **Grid Container** | [[Display grid]], [[Grid template columns]], [[Grid template rows]], [[Grid template areas]], [[Grid template shorthand]], [[Grid auto columns]], [[Grid auto rows]], [[Grid auto flow]], [[Grid gap]], [[Row gap]], [[Column gap]] |
| **Grid Items** | [[Grid column start]], [[Grid column end]], [[Grid column shorthand]], [[Grid row start]], [[Grid row end]], [[Grid row shorthand]], [[Grid area]], [[Named grid lines]], [[Span keyword]] |
| **Grid Alignment** | [[Justify items]], [[Align items]], [[Justify content]], [[Align content]], [[Justify self]], [[Align self]], [[Place items]], [[Place content]], [[Place self]] |
| **Grid Sizing** | [[Fr unit]], [[Minmax function]], [[Auto-fit keyword]], [[Auto-fill keyword]], [[Repeat function]], [[Grid min content]], [[Grid max content]], [[Fit content function]] |
| **Advanced Grid** | [[Implicit grid]], [[Explicit grid]], [[Grid layering]], [[Subgrid]], [[Nested grids]], [[Dense packing]], [[Grid line naming]], [[Named grid areas]] |
| **Grid Patterns** | [[Full-bleed layout]], [[Magazine layout]], [[Card grid]], [[Responsive grid]], [[Image gallery]], [[Dashboard layout]], [[Sidebar layout]] |

---
## [[#Table 9: Responsive Design|Table 9: Responsive Design]] 🟡→🔴

> `level:: intermediate-advanced` | `tags:: #css #responsive` | `status:: todo`

| Topic | Clickable Subtopics |
|-------|---------------------|
| **Media Queries** | [[Media query syntax]], [[Media types]], [[Screen media type]], [[Print media type]], [[Media features]], [[Width feature]], [[Height feature]], [[Min-width]], [[Max-width]], [[Min-height]], [[Max-height]], [[Orientation portrait]], [[Orientation landscape]], [[Aspect ratio]], [[Resolution]], [[Color scheme preference]], [[Reduced motion preference]], [[High contrast preference]], [[Pointer media feature]], [[Hover media feature]], [[Display mode]], [[Breakpoints]], [[Mobile-first approach]], [[Desktop-first approach]], [[Common breakpoints]], [[Custom breakpoints]], [[Breakpoint strategy]] |
| **Responsive Units** | [[Viewport width]], [[Viewport height]], [[Viewport min]], [[Viewport max]], [[Dynamic viewport units]], [[Small viewport units]], [[Large viewport units]], [[Container query units]] |
| **Responsive Images** | [[Max-width technique]], [[Object fit]], [[Object position]], [[Picture element support]], [[Art direction]] |
| **Container Queries** | [[Container type]], [[Container name]], [[Container query syntax]], [[Container units]], [[Container-based layouts]] |
| **Responsive Typography** | [[Fluid typography]], [[Clamp function]], [[Viewport-based sizing]], [[Responsive line height]], [[Responsive spacing]] |

---
## [[#Table 10: Transforms & Transitions|Table 10: Transforms & Transitions]] 🟡 Intermediate

> `level:: intermediate` | `tags:: #css #transforms #transitions` | `status:: todo`

| Topic | Clickable Subtopics |
|-------|---------------------|
| **2D Transforms** | [[Transform property]], [[Translate function]], [[Translate X]], [[Translate Y]], [[Scale function]], [[Scale X]], [[Scale Y]], [[Rotate function]], [[Skew function]], [[Skew X]], [[Skew Y]], [[Transform origin]], [[Multiple transforms]] |
| **3D Transforms** | [[Translate Z]], [[Translate 3D]], [[Scale Z]], [[Scale 3D]], [[Rotate X]], [[Rotate Y]], [[Rotate Z]], [[Rotate 3D]], [[Perspective]], [[Perspective origin]], [[Transform style]], [[Preserve 3D]], [[Backface visibility]] |
| **Transitions** | [[Transition property]], [[Transition duration]], [[Transition timing function]], [[Transition delay]], [[Transition shorthand]], [[Multiple transitions]], [[All properties transition]] |
| **Timing Functions** | [[Ease timing]], [[Linear timing]], [[Ease-in timing]], [[Ease-out timing]], [[Ease-in-out timing]], [[Cubic-bezier function]], [[Steps function]], [[Custom easing]] |

---
## [[#Table 11: Animations|Table 11: Animations]] 🔴 Advanced

> `level:: advanced` | `tags:: #css #animations` | `status:: todo`

| Topic | Clickable Subtopics |
|-------|---------------------|
| **Keyframes** | [[Keyframes rule]], [[Animation name]], [[From and to keywords]], [[Percentage keyframes]], [[Multiple keyframes]], [[Keyframe selectors]] |
| **Animation Properties** | [[Animation duration]], [[Animation timing function]], [[Animation delay]], [[Animation iteration count]], [[Animation direction]], [[Animation fill mode]], [[Animation play state]], [[Animation shorthand]] |
| **Animation Direction** | [[Normal direction]], [[Reverse direction]], [[Alternate direction]], [[Alternate reverse]] |
| **Animation Fill Mode** | [[None fill mode]], [[Forwards fill mode]], [[Backwards fill mode]], [[Both fill mode]] |
| **Advanced Animations** | [[Multiple animations]], [[Animation timeline]], [[Animation chaining]], [[Scroll-driven animations]], [[View timeline]], [[Animation range]], [[Performance optimization]], [[Will-change property]], [[GPU acceleration]], [[Animation best practices]] |

---
## [[#Table 12: Advanced Layout Techniques|Table 12: Advanced Layout Techniques]] 🔴 Advanced

> `level:: advanced` | `tags:: #css #advanced-layout` | `status:: todo`

| Topic | Clickable Subtopics |
|-------|---------------------|
| **Multi-column Layout** | [[Column count]], [[Column width]], [[Column gap]], [[Column rule]], [[Column rule width]], [[Column rule style]], [[Column rule color]], [[Column span]], [[Column fill]], [[Break inside]], [[Break before]], [[Break after]] |
| **Shapes** | [[Shape outside]], [[Shape margin]], [[Shape image threshold]], [[Circle shape]], [[Ellipse shape]], [[Polygon shape]], [[Inset shape]], [[Shape from image]] |
| **Clipping & Masking** | [[Clip path]], [[Clip rule]], [[Circle clip]], [[Ellipse clip]], [[Polygon clip]], [[Inset clip]], [[Mask image]], [[Mask mode]], [[Mask repeat]], [[Mask position]], [[Mask size]], [[Mask composite]] |
| **Blend Modes** | [[Mix blend mode]], [[Background blend mode]], [[Multiply blend]], [[Screen blend]], [[Overlay blend]], [[Darken blend]], [[Lighten blend]], [[Color-dodge blend]], [[Color-burn blend]], [[Hard-light blend]], [[Soft-light blend]], [[Difference blend]], [[Exclusion blend]], [[Hue blend]], [[Saturation blend]], [[Color blend]], [[Luminosity blend]] |
| **Filters** | [[Filter property]], [[Blur filter]], [[Brightness filter]], [[Contrast filter]], [[Drop shadow filter]], [[Grayscale filter]], [[Hue rotate filter]], [[Invert filter]], [[Opacity filter]], [[Saturate filter]], [[Sepia filter]], [[Multiple filters]], [[Backdrop filter]] |

---
## [[#Table 13: Modern CSS Features|Table 13: Modern CSS Features]] 🔴 Advanced

> `level:: advanced` | `tags:: #css #modern` | `status:: todo`

| Topic | Clickable Subtopics |
|-------|---------------------|
| **Custom Properties** | [[CSS variables]], [[Variable declaration]], [[Variable usage]], [[Variable scope]], [[Variable fallbacks]], [[Variable inheritance]], [[Variable with calc]], [[Dynamic variables]], [[JavaScript variable manipulation]] |
| **CSS Functions** | [[Calc function]], [[Min function]], [[Max function]], [[Clamp function]], [[Attr function]], [[Var function]], [[Url function]], [[Counter function]], [[Gradient functions]], [[Transform functions]], [[Filter functions]] |
| **Logical Properties** | [[Inline size]], [[Block size]], [[Inline start]], [[Inline end]], [[Block start]], [[Block end]], [[Margin inline]], [[Margin block]], [[Padding inline]], [[Padding block]], [[Border inline]], [[Border block]] |
| **CSS Grid Level 2** | [[Subgrid]], [[Masonry layout]] |
| **Cascade Layers** | [[Layer rule]], [[Layer ordering]], [[Layer unlayered]], [[Layer imports]] |
| **CSS Nesting** | [[Nesting selectors]], [[Nesting ampersand]], [[Nesting at-rules]], [[Nesting media queries]] |
| **Color Level 4 & 5** | [[Color mix function]], [[Relative colors]], [[Color contrast]], [[Accent color]] |
| **Has Selector** | [[Has pseudo-class]], [[Parent selector]], [[Previous sibling]] |
| **Scope** | [[Scope rule]], [[Scope root]], [[Scope boundaries]] |

---
## [[#Table 14: CSS Architecture|Table 14: CSS Architecture]] 🔴 Advanced

> `level:: advanced` | `tags:: #css #architecture` | `status:: todo`

| Topic | Clickable Subtopics |
|-------|---------------------|
| **Methodologies** | [[BEM naming]], [[Block element modifier]], [[OOCSS principles]], [[Object-oriented CSS]], [[SMACSS architecture]], [[Scalable modular architecture]], [[ITCSS structure]], [[Inverted triangle]], [[Atomic CSS]], [[Utility-first CSS]] |
| **Code Organization** | [[File structure]], [[Partial files]], [[Import strategy]], [[Component-based CSS]], [[Module organization]], [[Naming conventions]], [[Selector naming]], [[Class naming patterns]] |
| **CSS Preprocessors** | [[SASS/SCSS concepts]], [[LESS concepts]], [[Stylus concepts]], [[Variables in preprocessors]], [[Nesting]], [[Mixins]], [[Functions]], [[Extends]], [[Imports]], [[Partials]], [[Operators]] |
| **PostCSS** | [[PostCSS concepts]], [[Autoprefixer]], [[CSS modules]], [[PurgeCSS]], [[CSS optimization]] |
| **CSS-in-JS** | [[Styled components concept]], [[Emotion concept]], [[CSS modules in JS]], [[Runtime vs compile-time]] |
| **Design Systems** | [[Design tokens]], [[Component libraries]], [[Style guides]], [[Pattern libraries]], [[Documentation]] |

---
## [[#Table 15: Performance & Optimization|Table 15: Performance & Optimization]] 🔴 Advanced

> `level:: advanced` | `tags:: #css #performance` | `status:: todo`

| Topic | Clickable Subtopics |
|-------|---------------------|
| **Loading Performance** | [[Critical CSS]], [[Above-the-fold CSS]], [[Render-blocking CSS]], [[CSS loading strategies]], [[Defer non-critical CSS]], [[Inline critical CSS]], [[Preload stylesheets]] |
| **Selector Performance** | [[Selector efficiency]], [[Expensive selectors]], [[Universal selector cost]], [[Descendant selector cost]], [[Attribute selector cost]], [[Pseudo-class cost]], [[Right-to-left parsing]] |
| **Rendering Performance** | [[Paint optimization]], [[Layout thrashing]], [[Forced reflow]], [[Composite layers]], [[Hardware acceleration]], [[Transform optimization]], [[Opacity optimization]], [[Will-change optimization]], [[Contain property]], [[Content visibility]] |
| **CSS Optimization** | [[Minification]], [[Compression]], [[Unused CSS removal]], [[CSS purging]], [[CSS splitting]], [[Tree shaking]], [[Bundle size]] |
| **Debugging Performance** | [[DevTools profiling]], [[Paint flashing]], [[Layer borders]], [[Performance metrics]], [[First contentful paint]], [[Largest contentful paint]], [[Cumulative layout shift]] |

---
## [[#Table 16: Accessibility & Best Practices|Table 16: Accessibility & Best Practices]] 🟡→🔴

> `level:: intermediate-advanced` | `tags:: #css #a11y #best-practices` | `status:: todo`

| Topic | Clickable Subtopics |
|-------|---------------------|
| **Visual Accessibility** | [[Color contrast ratios]], [[WCAG AA standards]], [[WCAG AAA standards]], [[Text readability]], [[Minimum font sizes]], [[Line length]], [[Line height for readability]], [[Focus indicators]], [[Focus visible styles]], [[Focus outlines]], [[Skip to content]] |
| **Motion Accessibility** | [[Prefers reduced motion]], [[Safe animations]], [[Vestibular disorders]], [[Animation alternatives]] |
| **Screen Reader Support** | [[Visually hidden content]], [[Screen reader only styles]], [[Hidden vs display none]], [[Visibility vs opacity]] |
| **Print Styles** | [[Print media query]], [[Print-specific styles]], [[Page break properties]], [[Print color adjustment]], [[Print background]] |
| **CSS Best Practices** | [[Maintainable CSS]], [[Scalable CSS]], [[Reusable components]], [[DRY principle]], [[Separation of concerns]], [[Mobile-first CSS]], [[Progressive enhancement]], [[Graceful degradation]], [[Browser support strategy]], [[Vendor prefixes]], [[Feature detection]], [[Fallback styles]] |
| **Code Quality** | [[CSS validation]], [[W3C validator]], [[Linting]], [[Code formatting]], [[Consistent indentation]], [[Comment usage]], [[Documentation]] |
