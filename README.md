# renderer-w-three-r
I'll keep my progress of making a renderer.

## Preliminary:
1) <a href="https://raytracing.github.io/books/RayTracingInOneWeekend.html">Ray Tracing in One Weekend</a>
2) <a href="https://raytracing.github.io/books/RayTracingTheNextWeek.html">Ray Tracing The Next Week</a>
3) <a href="https://raytracing.github.io/books/RayTracingTheRestOfYourLife.html">Ray Tracing The Rest Of Your Life</a>
4) <a href="https://research.quanfita.cn/files/Physically_Based_Rendering_Third_Edition.pdf">Physically Based Renderer</a>

I'm following advices from this <a href="https://in1weekend.blogspot.com/">Peter Shirley's blog</a>.

---

# Ray Tracing in One Weekend

I've already done <a href="https://raytracing.github.io/books/RayTracingInOneWeekend.html">Ray Tracing in One Weekend</a> before, so it's a good refresher.

## To run this:
`g++ -Wall -std=c++14 src/inOneWeekend/main.cpp -o build/inOneWeekend`

## then:
`build/inOneWeekend > results/inOneWeekend/image.ppm`

## Antialiasing 

Before antialiasing:

<img src="https://github.com/jhruvsphysics/renderer-w-three-r/blob/main/results/inOneWeekend/2normalwground.jpg">

After antialiasing:

<img src="https://github.com/jhruvsphysics/renderer-w-three-r/blob/main/results/inOneWeekend/3antialiasing.jpg">

## Diffuse/Lambertian

In order to generate a random scattered rays at point p, we first get a vector in unit sphere via rejection method. We normalize that vector to sit on the boundary of unit sphere. Then,

` p + normal_vec + random_unit_vector = scattered ray`

<img src="https://github.com/jhruvsphysics/renderer-w-three-r/blob/main/results/inOneWeekend/diffuse.jpg">

## Metal

For a metal material, the scattered ray would be reflected.

Here, we have a scene with one metal ball, two lambertian balls on a metal surface.

<img src="https://github.com/jhruvsphysics/renderer-w-three-r/blob/main/results/inOneWeekend/11lambertianmetal.jpg">

### Fuzzy Reflection

For a fuzzy metal material, we add a vector in unit sphere to the scattered/reflected ray.
