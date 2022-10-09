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

<img src="https://github.com/jhruvsphysics/renderer-w-three-r/blob/main/results/inOneWeekend/12fuzz.jpg">

## Refraction

Snell's law state that for a incoming ray $R$ onto a surface with normal $\hat n$ making an angle $\theta$, and for a refracted ray $R'$ with $\theta'$:

$$\sin\theta' = \eta/\eta' \sin\theta$$

In order to get the refracted ray, we see that 

$$R' = R_{\perp}' + R_{\parallel}'$$

Without loss of generality, assume the direction of $R_{\perp}'$ is $\hat x$ and that $|R'| = |R| = 1$, they are unit directional vector.

$$\Rightarrow R_{\perp}' = |R'|\sin\theta' \hat x$$

$$\Rightarrow R_{\perp}' = |R|\frac{\eta}{\eta'}\sin\theta \hat x$$

Note that, geometrically it's trivial to see that 

$$|R|\sin\theta \hat x = R + \cos\theta \hat n$$

Algebraically, we can use cross product to show this, where $\lambda$ is some constant:

$$\lambda \hat x = (\hat n \times R) \times \hat n = R(\hat n \cdot \hat n) - (\hat n \cdot R)\hat n= R - (\hat n \cdot R)\hat n$$

$$\Rightarrow \lambda^2 \hat x^2 = \lambda^2 = R^2 - 2(\hat n \cdot R)^2 + (\hat n \cdot R)^2$$

$$\Rightarrow \lambda^2 = 1 - (\hat n \cdot R)^2 = 1 - \cos^2\theta = \sin^2\theta$$

$$\Rightarrow \lambda = \sin\theta$$

$$\therefore \sin\theta \hat x = R - (\hat n \cdot R)\hat n = R + \cos\theta \hat n$$

Hence,

$$\Rightarrow R_{\perp}' = \frac{\eta}{\eta'}(R + \cos\theta \hat n)$$

$$\Rightarrow R_{\parallel}' = - \sqrt{1 - |R_{\perp}'|^2} \hat n$$

We get the following image for $\eta' = 1.5$ (glass),

<img src="https://github.com/jhruvsphysics/renderer-w-three-r/blob/main/results/inOneWeekend/14dielectric_demo.jpg">
