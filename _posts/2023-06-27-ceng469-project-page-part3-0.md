# Ceng469 Final Project: Temporal Tone Mapping for Simulating Human Visual System: Part 3: Results

My project aims to model how the Human Visual System (HVS) adjusts to cahnges in light level. I plan to achieve this by using temporal tonemapping operators. To be more spesific, I will be modifying Reinhard's Global Operator as a rolling average between frames, both as a aritmatic and geometric average to compare the results.

## Navigation

## Results

### Test Enviroment

#### Scene

To keep my test cases through, I tested all cases in a very simple scene. It consists of a closed room, with a large central model, which is the standart armadillo model for out case. In each side of the central model lies a light source, which is repesented by spheres. Finally, also in each side of the central figure lies small models, which are utah teapots for this demonstration, placed in a way that the central would cast shadows on them. Left light source is always on in all cases with a luminance value of $$1$$. When it is on, right light source has a luminance value of $$512$$.

#### Variables

I worked with 4 scenerios.

1. Right light is turned on at frame 30, and stayed on until the end of the scene.
2. Right light is turned off at the frame 30 and stayed off untill the end of the scene.
3. Right light is turned on at frame 30 and turned back off at frame 60.
4. Right light is turned off at frame 30 and turned back on at frame 60.

First 2 cases are designed to simulate changes in the light level, while the other 2 cases are designed to simulate a sudden flash of light or darkness.

For all cases, I worked with 20 different values for the weight $$w$$ used in $$\text{avg}_x$$ function. In comparison videos, they are situated as such:

| $$0.005$$ | $$0.010$$ | $$0.020$$ | $$0.030$$ | $$0.040$$ |
| $$0.050$$ | $$0.060$$ | $$0.070$$ | $$0.080$$ | $$0.090$$ |
| $$0.100$$ | $$0.200$$ | $$0.300$$ | $$0.400$$ | $$0.500$$ |
| $$0.600$$ | $$0.700$$ | $$0.800$$ | $$0.900$$ | $$1.000$$ |

#### Video Comparison

While calculating convergence speed (the frame where the render converges to its final value), I took the Manhattan Distance between the last frame of the case with $$w=1.000$$ as for that value of $$w$$, the frame would converge to its final form instantly. In addition, I printed the time frames where the Manhattan Distance is 600*600 (size of the frames), 600, 60, and 0 as a table. The table has the same structure as the videos, like:

| $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ |
| $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ |
| $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ |
| $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ |

Because of the nature of text scenerios, the first possible frame for convergence is 31 for the case of scenerio 1 and 2, and 61 for scenerio 3 and 4. And because all tests are done in 30 FPS over a duration of 1 minute, with the index of first frame being 0, a value of 1799 is probably means it does not converge in 1 minutes instead of it converged in the last possible frame.
