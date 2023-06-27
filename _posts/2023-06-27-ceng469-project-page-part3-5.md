# Ceng469 Final Project: Temporal Tone Mapping for Simulating Human Visual System: Part 3.5: Comparing Results

My project aims to model how the Human Visual System (HVS) adjusts to cahnges in light level. I plan to achieve this by using temporal tonemapping operators. To be more spesific, I will be modifying Reinhard's Global Operator as a rolling average between frames, both as a aritmatic and geometric average to compare the results.

## Navigation

1. <a href="{{site.url}}/2023/06/27/ceng469-project-page-part1">Part 1: Theory</a>
2. <a href="{{site.url}}/2023/06/27/ceng469-project-page-part2">Part 2: Implementation</a>
3. <a href="{{site.url}}/2023/06/27/ceng469-project-page-part3-0">Part 3: Results</a>
   1. <a href="{{site.url}}/2023/06/27/ceng469-project-page-part3-1">Part 3.1: Formula 1</a>
   2. <a href="{{site.url}}/2023/06/27/ceng469-project-page-part3-2">Part 3.2: Formula 2</a>
   3. <a href="{{site.url}}/2023/06/27/ceng469-project-page-part3-3">Part 3.3: Formula 3</a>
   4. <a href="{{site.url}}/2023/06/27/ceng469-project-page-part3-4">Part 3.4: Formula 4</a>
   5. <a href="{{site.url}}/2023/06/27/ceng469-project-page-part3-5">Part 3.5: Comparing Results</a>
4. <a href="{{site.url}}/2023/06/27/ceng469-project-page-part4">Final Remarks</a>

## Results

The 4 formulas I tried can be divided in 2 categories, Formulas 1 and 2 use aritmatic averaging while Formulas 3 and 4 use geometric averaging. As I discuss previously, the aritmatic averaging formulas are defined by their asymetry while geometric formulas are symetric, and also there is no practical difference between the results of geometric formulas. While dynamic weight selection would be easier to implement in symetric cases, I found the results obtained by the asymetric algorithms more interesting to look at. Probably a dynamic weight selection approach implemented using symetric algorithms that would mimic the asymetry of the first two formulas in certain cases would result in the best results.

In all cases, in term of weight selection, the results obtained by weight between $$0.01$$ and $$0.03$$ produce the most appealing results in my opinion. Any higher weights converges nearly instantly, making the visual effect provided by this method nearly insignificant. Any lower weights results in very great convergence times, in some cases even exceeding the bounds of my testing.

In its current form, no configuration or formula produces results that are very satisfactory in the case of flashing lights. This is the be expected due to the fact that no special arrangement is done to simulate them spesifically. But in the case of regular scenes with triggers that changes the light level dramatically, I find the results I obtained excelent for most of the cases, even without any arrangement for dynamically choosing weights. 