# Ceng469 Final Project: Temporal Tone Mapping for Simulating Human Visual System

My project aims to model how the Human Visual System (HVS) adjusts to cahnges in light level. I plan to achieve this by using temporal tonemapping operators. To be more spesific, I will be modifying Reinhard's Global Operator as a rolling average between frames, both as a aritmatic and geometric average to compare the results.

## Backgroud

### Tone Mapping

Tone Mapping Operators (TMO) are mathematical operators that maps High Dynamic Range (HDR) data to Low Dynamic Range (LDR) data, generaly for the purpose of displaying it in LDR monitors.

### Temporal Tone Mapping

Temporal Tone Mapping uses TMOs that uses information contained in not only the current frame, but also the previous (and possibly upcomming) frames. The aim of Temporal TMOs are generally one or more of the following

* Preventing flickering between frames.
* Preserving color of objects between frames.
* Preserving the contrast of the image between frames.

I will be using a Temporal TMO of my design to simulate the HVS.

### Reinhard's Global Operator

Reinhard's Global Operator uses a log-average luminance as the approximation of a scene, called $\overline{L}_w$ or average white. This value is computed by \[\overline{L}_w = \frac{1}{N} \exp{\left(\sum_{x,y}\log{(\epsilon + L_w (x,y)} \right)}\] where $L_w (x,y)$ is the world lumanace of the pixel $(x, y)$, generally calculated as $\text{Red}(x,y) \times 0.2126 + \text{Green}(x,y) \times 0.7152 + \text{Blue}(x,y) \times 0.0722$ and $\epsilon$ is a very small value. By using this formula we can calculate the scales luminance of $(x,y)$ as $L(x,y)$ which is equal to $$L(x,y)=\frac{\alpha}{\overline{L}_w}L_w(x,y).$$ $\alpha$ is what the log-average is mapped to, which is usually $\alpha = 0.18$. Finally we compress the scaled luminance to LDR values using the formula $$L_d(x,y)=\frac{L(x,y)}{1 + L(x,y)}.$$

## My Temporal TMO

I tried 4 formulas for my project, 2 of which used aritmatic weighted average and 2 of which used geometric weighted average. If $\overline{L}_w(i)$ is the avarage white of the frame $i$ and $\dot{L}_w(i)$ is the calculated average white of frame $i$, that is actually used in rendering the frame, $w$ is the weight of the current frame, $\text{av}_x(w, \underline{L}_w, \dot{L}_w)$ is averaging function of type $x$ (where $\underline{L}_w$ has the weight $w$ and $\dot{L}_w$ has the weight $1-w$), and finally $\text{Rend}(X)$ calculated $L(x,y)$ as $$L(x,y) = X \times L_w(x, y),$$ my formulas can be described as the following.

### Formula 1
```math
\begin{align*}
\overline{L}_w(i) &= \dfrac{\alpha}{\exp{\left(\dfrac{\sum_{x, y} \log{(\epsilon + L_w(x,y,i))}}{N} \right)}}\\
\dot{L}_w(i) &= \begin{cases}\ \overline{L}_w(i)&, i=0\\ \text{av}_1(w, \overline{L}_w(i), \dot{L}_w(i-1))&, i \neq 0  \end{cases}\\
\text{av}_1(w, \overline{L}_w, \dot{L}_w) &= w * \overline{L}_w + (1-w)\dot{L}_w\\
\text{Rend}&(\dot{L}_w(i))
\end{align*}
```
### Formula 2
```math
\begin{align*}
\overline{L}_w(i) &= \exp{\left(\dfrac{\sum_{x, y} \log{(\epsilon + L_w(x,y,i))}}{N} \right)}\\
\dot{L}_w(i) &= \begin{cases}\ \overline{L}_w(i)&, i=0\\ \text{av}_2(w, \overline{L}_w(i), \dot{L}_w(i-1))&, i \neq 0  \end{cases}\\
\text{av}_2(w, \overline{L}_w, \dot{L}_w) &= w * \overline{L}_w + (1-w)\dot{L}_w\\
\text{Rend}&\left(\frac{\alpha}{\dot{L}_w(i)}\right)
\end{align*}
```
### Formula 3
```math
\begin{align*}
\overline{L}_w(i) &= \dfrac{\alpha}{\exp{\left(\dfrac{\sum_{x, y} \log{(\epsilon + L_w(x,y,i))}}{N} \right)}}\\
\dot{L}_w(i) &= \begin{cases}\ \overline{L}_w(i)&, i=0\\ \text{av}_3(w, \overline{L}_w(i), \dot{L}_w(i-1))&, i \neq 0  \end{cases}\\
\text{av}_3(w, \overline{L}_w, \dot{L}_w) &= \exp{( w * \log{\overline{L}_w} + (1-w)\log{\dot{L}_w})}\\
\text{Rend}&(\dot{L}_w(i))
\end{align*}
```
### Formula 4
```math
\begin{align*}
\overline{L}_w(i) &= \exp{\left(\dfrac{\sum_{x, y} \log{(\epsilon + L_w(x,y,i))}}{N} \right)}\\
\dot{L}_w(i) &= \begin{cases}\ \overline{L}_w(i)&, i=0\\ \text{av}_4(w, \overline{L}_w(i), \dot{L}_w(i-1))&, i \neq 0  \end{cases}\\
\text{av}_4(w, \overline{L}_w, \dot{L}_w) &= \exp{( w * \log{\overline{L}_w} + (1-w)\log{\dot{L}_w})}\\
\text{Rend}&\left(\frac{\alpha}{\dot{L}_w(i)}\right)
\end{align*}
```
## Implementation

## Results

### Test Enviroment

#### Scene

#### Variables

### Video Comperasion

### Convergence Speed

## Final Remarks
