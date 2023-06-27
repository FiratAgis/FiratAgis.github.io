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

Reinhard's Global Operator uses a log-average luminance as the approximation of a scene, called $$\overline{L}_w$$ or average white. This value is computed by 

$$\overline{L}_w = \frac{1}{N} \exp{\left(\sum_{x,y}\log{(\epsilon + L_w (x,y)} \right)}$$

where $$L_w (x,y)$$ is the world lumanace of the pixel $$(x, y)$$, generally calculated as $$\text{Red}(x,y) \times 0.2126 + \text{Green}(x,y) \times 0.7152 + \text{Blue}(x,y) \times 0.0722$$ and $$\epsilon$$ is a very small value. By using this formula we can calculate the scales luminance of $$(x,y)$$ as $$L(x,y)$$ which is equal to 

$$L(x,y)=\frac{\alpha}{\overline{L}_w}L_w(x,y).$$ 

$\alpha$ is what the log-average is mapped to, which is usually $$\alpha = 0.18$$. Finally we compress the scaled luminance to LDR values using the formula

$$L_d(x,y)=\frac{L(x,y)}{1 + L(x,y)}.$$

## My Temporal TMO

I tried 4 formulas for my project, 2 of which used aritmatic weighted average and 2 of which used geometric weighted average. If $$\overline{L}_w(i)$$ is the avarage white of the frame $$i$$ and $$\dot{L}_w(i)$$ is the calculated average white of frame $$i$$, that is actually used in rendering the frame, $$w$$ is the weight of the current frame, $$\text{av}_x(w, \overline{L}_w, \dot{L}_w)$$ is averaging function of type $$x$$ (where $$\overline{L}_w$$ has the weight $$w$$ and $$\dot{L}_w$$ has the weight $$1-w$$), and finally $$\text{Rend}(X)$$ calculated $$L(x,y)$$ as 

$$L(x,y) = X \times L_w(x, y),$$

my formulas can be described as the following.

### Formula 1

$$\begin{align*}
\overline{L}_w(i) &= \dfrac{\alpha}{\exp{\left(\dfrac{\sum_{x, y} \log{(\epsilon + L_w(x,y,i))}}{N} \right)}}\\
\dot{L}_w(i) &= \begin{cases}\ \overline{L}_w(i)&, i=0\\ \text{av}_1(w, \overline{L}_w(i), \dot{L}_w(i-1))&, i \neq 0  \end{cases}\\
\text{av}_1(w, \overline{L}_w, \dot{L}_w) &= w * \overline{L}_w + (1-w)\dot{L}_w\\
\text{Rend}&(\dot{L}_w(i))
\end{align*}$$

### Formula 2

$$\begin{align*}
\overline{L}_w(i) &= \exp{\left(\dfrac{\sum_{x, y} \log{(\epsilon + L_w(x,y,i))}}{N} \right)}\\
\dot{L}_w(i) &= \begin{cases}\ \overline{L}_w(i)&, i=0\\ \text{av}_2(w, \overline{L}_w(i), \dot{L}_w(i-1))&, i \neq 0  \end{cases}\\
\text{av}_2(w, \overline{L}_w, \dot{L}_w) &= w * \overline{L}_w + (1-w)\dot{L}_w\\
\text{Rend}&\left(\frac{\alpha}{\dot{L}_w(i)}\right)
\end{align*}$$

### Formula 3

$$\begin{align*}
\overline{L}_w(i) &= \dfrac{\alpha}{\exp{\left(\dfrac{\sum_{x, y} \log{(\epsilon + L_w(x,y,i))}}{N} \right)}}\\
\dot{L}_w(i) &= \begin{cases}\ \overline{L}_w(i)&, i=0\\ \text{av}_3(w, \overline{L}_w(i), \dot{L}_w(i-1))&, i \neq 0  \end{cases}\\
\text{av}_3(w, \overline{L}_w, \dot{L}_w) &= \exp{( w * \log{\overline{L}_w} + (1-w)\log{\dot{L}_w})}\\
\text{Rend}&(\dot{L}_w(i))
\end{align*}$$

### Formula 4

$$\begin{align*}
\overline{L}_w(i) &= \exp{\left(\dfrac{\sum_{x, y} \log{(\epsilon + L_w(x,y,i))}}{N} \right)}\\
\dot{L}_w(i) &= \begin{cases}\ \overline{L}_w(i)&, i=0\\ \text{av}_4(w, \overline{L}_w(i), \dot{L}_w(i-1))&, i \neq 0  \end{cases}\\
\text{av}_4(w, \overline{L}_w, \dot{L}_w) &= \exp{( w * \log{\overline{L}_w} + (1-w)\log{\dot{L}_w})}\\
\text{Rend}&\left(\frac{\alpha}{\dot{L}_w(i)}\right)
\end{align*}$$

## Implementation

## Results

### Test Enviroment

#### Scene

To keep my test cases through, I tested all cases in a very simple scene. It consists of a closed room, with a large central model, which is the standart armadillo model for out case. In each side of the central model lies a light source, which is repesented by spheres. Finally, also in each side of the central figure lies small models, which are utah teapots for this demonstration, placed in a way that the central would cast shadows on them. Left light source is always on in all cases with a luminance value of 1. When it is on, right light source has a luminance value of $$512$$.

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

### Video Comparison

While calculating convergence speed (the frame where the render converges to its final value), I took the Manhattan Distance between the last frame of the case with $$w=1.000$$ as for that value of $$w$$, the frame would converge to its final form instantly. In addition, I printed the time frames where the Manhattan Distance is 600*600 (size of the frames), 600, 60, and 0 as a table. The table has the same structure as the videos, like:

| $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ |
| $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ |
| $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ |
| $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ |

#### Case 1 Formula 1

<iframe width="1000" height="800" src="https://www.youtube.com/embed/0su4lINw-Ig" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

| $$\begin{align*}&1799\\&1799\\&1799\\&1799\end{align*}$$ | $$\begin{align*}&1063\\&1643\\&1744\\&1779\end{align*}$$ | $$\begin{align*}&544\\&833\\&884\\&897\end{align*}$$ | $$\begin{align*}&371\\&560\\&590\\&597\end{align*}$$ | $$\begin{align*}&285\\&425\\&446\\&450\end{align*}$$ |
| $$\begin{align*}&233\\&345\\&362\\&366\end{align*}$$ | $$\begin{align*}&198\\&291\\&306\\&309\end{align*}$$ | $$\begin{align*}&173\\&253\\&266\\&269\end{align*}$$ | $$\begin{align*}&155\\&224\\&235\\&237\end{align*}$$ | $$\begin{align*}&140\\&201\\&211\\&213\end{align*}$$ |
| $$\begin{align*}&129\\&183\\&192\\&194\end{align*}$$ | $$\begin{align*}&77\\&103\\&107\\&108\end{align*}$$ | $$\begin{align*}&60\\&76\\&78\\&79\end{align*}$$ | $$\begin{align*}&51\\&62\\&64\\&64\end{align*}$$ | $$\begin{align*}&45\\&54\\&55\\&55\end{align*}$$ |
| $$\begin{align*}&42\\&48\\&49\\&49\end{align*}$$ | $$\begin{align*}&39\\&44\\&45\\&45\end{align*}$$ | $$\begin{align*}&37\\&41\\&41\\&41\end{align*}$$ | $$\begin{align*}&35\\&38\\&38\\&38\end{align*}$$ | $$\begin{align*}&31\\&31\\&31\\&31\end{align*}$$ |

#### Case 2 Formula 1

<iframe width="1000" height="800" src="https://www.youtube.com/embed/mDigGMG7B7o" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

| $$\begin{align*}&976\\&1799\\&1799\\&1799\end{align*}$$ | $$\begin{align*}&502\\&1108\\&1184\\&1207\end{align*}$$ | $$\begin{align*}&265\\&566\\&602\\&620\end{align*}$$ | $$\begin{align*}&186\\&384\\&406\\&416\end{align*}$$ | $$\begin{align*}&147\\&297\\&315\\&324\end{align*}$$ |
| $$\begin{align*}&123\\&242\\&257\\&263\end{align*}$$ | $$\begin{align*}&107\\&206\\&218\\&223\end{align*}$$ | $$\begin{align*}&96\\&180\\&190\\&195\end{align*}$$ | $$\begin{align*}&87\\&160\\&169\\&173\end{align*}$$ | $$\begin{align*}&81\\&146\\&154\\&158\end{align*}$$ |
| $$\begin{align*}&75\\&134\\&141\\&144\end{align*}$$ | $$\begin{align*}&52\\&79\\&83\\&84\end{align*}$$ | $$\begin{align*}&44\\&61\\&63\\&64\end{align*}$$ | $$\begin{align*}&40\\&52\\&53\\&54\end{align*}$$ | $$\begin{align*}&37\\&46\\&47\\&48\end{align*}$$ |
| $$\begin{align*}&36\\&42\\&43\\&44\end{align*}$$ | $$\begin{align*}&34\\&40\\&40\\&40\end{align*}$$ | $$\begin{align*}&33\\&37\\&38\\&38\end{align*}$$ | $$\begin{align*}&33\\&35\\&36\\&36\end{align*}$$ | $$\begin{align*}&31\\&31\\&31\\&31\end{align*}$$ |

#### Case 3 Formula 1

<video width="1000" height="800" controls>
  <source src="{{site.url}}/videos/case_3_form_1.mp4" type="video/mp4">
Your browser does not support the video tag.
</video>

| $$\begin{align*}&564\\&1690\\&1777\\&1799\end{align*}$$ | $$\begin{align*}&387\\&993\\&1069\\&1092\end{align*}$$ | $$\begin{align*}&254\\&554\\&590\\&608\end{align*}$$ | $$\begin{align*}&198\\&369\\&419\\&428\end{align*}$$ | $$\begin{align*}&168\\&318\\&336\\&345\end{align*}$$ |
| $$\begin{align*}&148\\&267\\&282\\&289\end{align*}$$ | $$\begin{align*}&134\\&233\\&245\\&250\end{align*}$$ | $$\begin{align*}&124\\&208\\&218\\&223\end{align*}$$ | $$\begin{align*}&116\\&289\\&198\\&202\end{align*}$$ | $$\begin{align*}&110\\&175\\&183\\&187\end{align*}$$ |
| $$\begin{align*}&105\\&163\\&171\\&174\end{align*}$$ | $$\begin{align*}&82\\&109\\&113\\&114\end{align*}$$ | $$\begin{align*}&74\\&91\\&93\\&94\end{align*}$$ | $$\begin{align*}&70\\&82\\&83\\&84\end{align*}$$ | $$\begin{align*}&67\\&76\\&77\\&78\end{align*}$$ |
| $$\begin{align*}&62\\&72\\&73\\&74\end{align*}$$ | $$\begin{align*}&64\\&70\\&70\\&70\end{align*}$$ | $$\begin{align*}&63\\&67\\&68\\&68\end{align*}$$ | $$\begin{align*}&63\\&65\\&66\\&66\end{align*}$$ | $$\begin{align*}&61\\&61\\&61\\&61\end{align*}$$ |

#### Case 4 Formula 1

<video width="1000" height="800" controls>
  <source src="{{site.url}}/videos/case_4_form_1.mp4" type="video/mp4">
Your browser does not support the video tag.
</video>

| $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ |
| $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ |
| $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ |
| $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ |

#### Remarks on Formula 1

#### Case 1 Formula 2

<iframe width="1000" height="800" src="https://www.youtube.com/embed/ySNwRwLw_es" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

| $$\begin{align*}&975\\&1799\\&1799\\&1799\end{align*}$$ | $$\begin{align*}&502\\&1070\\&1154\\&1179\end{align*}$$ | $$\begin{align*}&265\\&548\\&590\\&599\end{align*}$$ | $$\begin{align*}&186\\&374\\&402\\&408\end{align*}$$ | $$\begin{align*}&146\\&288\\&311\\&316\end{align*}$$ |
| $$\begin{align*}&123\\&235\\&253\\&257\end{align*}$$ | $$\begin{align*}&107\\&200\\&215\\&218\end{align*}$$ | $$\begin{align*}&96\\&175\\&187\\&190\end{align*}$$ | $$\begin{align*}&87\\&156\\&167\\&169\end{align*}$$ | $$\begin{align*}&81\\&142\\&152\\&154\end{align*}$$ |
| $$\begin{align*}&75\\&130\\&139\\&141\end{align*}$$ | $$\begin{align*}&52\\&77\\&81\\&82\end{align*}$$ | $$\begin{align*}&44\\&60\\&62\\&63\end{align*}$$ | $$\begin{align*}&40\\&51\\&53\\&53\end{align*}$$ | $$\begin{align*}&37\\&46\\&47\\&47\end{align*}$$ |
| $$\begin{align*}&36\\&42\\&43\\&43\end{align*}$$ | $$\begin{align*}&34\\&39\\&40\\&40\end{align*}$$ | $$\begin{align*}&33\\&37\\&38\\&38\end{align*}$$ | $$\begin{align*}&33\\&35\\&35\\&36\end{align*}$$ | $$\begin{align*}&31\\&31\\&31\\&31\end{align*}$$ |

#### Case 2 Formula 2

<iframe width="1000" height="800" src="https://www.youtube.com/embed/5JUWPGsopaI" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

| $$\begin{align*}&1799\\&1799\\&1799\\&1799\end{align*}$$ | $$\begin{align*}&1060\\&1690\\&1790\\&1799\end{align*}$$ | $$\begin{align*}&542\\&843\\&878\\&894\end{align*}$$ | $$\begin{align*}&370\\&572\\&598\\&611\end{align*}$$ | $$\begin{align*}&284\\&434\\&452\\&460\end{align*}$$ |
| $$\begin{align*}&232\\&352\\&367\\&375\end{align*}$$ | $$\begin{align*}&198\\&296\\&308\\&313\end{align*}$$ | $$\begin{align*}&173\\&257\\&268\\&273\end{align*}$$ | $$\begin{align*}&155\\&228\\&238\\&242\end{align*}$$ | $$\begin{align*}&140\\&205\\&213\\&217\end{align*}$$ |
| $$\begin{align*}&129\\&187\\&194\\&197\end{align*}$$ | $$\begin{align*}&77\\&104\\&108\\&109\end{align*}$$ | $$\begin{align*}&59\\&77\\&79\\&80\end{align*}$$ | $$\begin{align*}&51\\&63\\&64\\&65\end{align*}$$ | $$\begin{align*}&45\\&54\\&55\\&56\end{align*}$$ |
| $$\begin{align*}&42\\&48\\&49\\&50\end{align*}$$ | $$\begin{align*}&39\\&44\\&45\\&45\end{align*}$$ | $$\begin{align*}&37\\&41\\&41\\&41\end{align*}$$ | $$\begin{align*}&35\\&38\\&38\\&38\end{align*}$$ | $$\begin{align*}&31\\&31\\&31\\&31\end{align*}$$ |

#### Case 3 Formula 2

<iframe src="https://drive.google.com/file/d/12mIWfiYW0-t4-W3jmgF4fnNo6CW5YNvM/preview" width="1000" height="800" allow="autoplay"></iframe>

| $$\begin{align*}&1731\\&1799\\&1799\\&1799\end{align*}$$ | $$\begin{align*}&956\\&1586\\&1686\\&1799\end{align*}$$ | $$\begin{align*}&533\\&834\\&869\\&885\end{align*}$$ | $$\begin{align*}&383\\&585\\&611\\&624\end{align*}$$ | $$\begin{align*}&305\\&455\\&473\\&481\end{align*}$$ |
| $$\begin{align*}&257\\&377\\&392\\&400\end{align*}$$ | $$\begin{align*}&225\\&324\\&335\\&340\end{align*}$$ | $$\begin{align*}&201\\&286\\&296\\&301\end{align*}$$ | $$\begin{align*}&184\\&257\\&267\\&271\end{align*}$$ | $$\begin{align*}&170\\&235\\&243\\&246\end{align*}$$ |
| $$\begin{align*}&158\\&216\\&223\\&226\end{align*}$$ | $$\begin{align*}&107\\&134\\&138\\&139\end{align*}$$ | $$\begin{align*}&89\\&107\\&109\\&110\end{align*}$$ | $$\begin{align*}&81\\&93\\&94\\&95\end{align*}$$ | $$\begin{align*}&75\\&84\\&85\\&86\end{align*}$$ |
| $$\begin{align*}&72\\&78\\&79\\&80\end{align*}$$ | $$\begin{align*}&69\\&74\\&75\\&75\end{align*}$$ | $$\begin{align*}&67\\&71\\&71\\&71\end{align*}$$ | $$\begin{align*}&65\\&68\\&68\\&68\end{align*}$$ | $$\begin{align*}&61\\&61\\&61\\&61\end{align*}$$ |

#### Case 4 Formula 2

<video width="1000" height="800" controls>
  <source src="{{site.url}}/videos/case_4_form_2.mp4" type="video/mp4">
Your browser does not support the video tag.
</video>

| $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ |
| $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ |
| $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ |
| $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ |

#### Remarks on Formula 2

#### Case 1 Formula 3

<iframe width="1000" height="800" src="https://www.youtube.com/embed/WKaipqUgVxo" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

| $$\begin{align*}&1321\\&1799\\&1799\\&1799\end{align*}$$ | $$\begin{align*}&674\\&1212\\&1247\\&1799\end{align*}$$ | $$\begin{align*}&350\\&616\\&644\\&648\end{align*}$$ | $$\begin{align*}&243\\&434\\&467\\&1799\end{align*}$$ | $$\begin{align*}&189\\&336\\&1799\\&1799\end{align*}$$ |
| $$\begin{align*}&157\\&271\\&292\\&296\end{align*}$$ | $$\begin{align*}&135\\&228\\&242\\&246\end{align*}$$ | $$\begin{align*}&120\\&198\\&209\\&211\end{align*}$$ | $$\begin{align*}&108\\&176\\&184\\&186\end{align*}$$ | $$\begin{align*}&99\\&162\\&174\\&178\end{align*}$$ |
| $$\begin{align*}&92\\&147\\&157\\&159\end{align*}$$ | $$\begin{align*}&59\\&85\\&89\\&90\end{align*}$$ | $$\begin{align*}&49\\&65\\&67\\&68\end{align*}$$ | $$\begin{align*}&43\\&54\\&56\\&56\end{align*}$$ | $$\begin{align*}&40\\&48\\&49\\&50\end{align*}$$ |
| $$\begin{align*}&38\\&44\\&45\\&45\end{align*}$$ | $$\begin{align*}&36\\&41\\&41\\&42\end{align*}$$ | $$\begin{align*}&35\\&38\\&39\\&39\end{align*}$$ | $$\begin{align*}&33\\&36\\&36\\&36\end{align*}$$ | $$\begin{align*}&31\\&31\\&31\\&31\end{align*}$$ |

#### Case 2 Formula 3

<iframe width="1000" height="800" src="https://www.youtube.com/embed/nDYNq5cXMRQ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

| $$\begin{align*}&1320\\&1799\\&1799\\&1799\end{align*}$$ | $$\begin{align*}&674\\&1284\\&1363\\&1386\end{align*}$$ | $$\begin{align*}&350\\&654\\&690\\&712\end{align*}$$ | $$\begin{align*}&243\\&444\\&469\\&481\end{align*}$$ | $$\begin{align*}&189\\&339\\&358\\&367\end{align*}$$ |
| $$\begin{align*}&156\\&276\\&291\\&298\end{align*}$$ | $$\begin{align*}&135\\&234\\&246\\&252\end{align*}$$ | $$\begin{align*}&120\\&204\\&214\\&219\end{align*}$$ | $$\begin{align*}&108\\&182\\&191\\&195\end{align*}$$ | $$\begin{align*}&99\\&164\\&172\\&176\end{align*}$$ |
| $$\begin{align*}&92\\&150\\&157\\&161\end{align*}$$ | $$\begin{align*}&59\\&87\\&90\\&92\end{align*}$$ | $$\begin{align*}&49\\&66\\&68\\&69\end{align*}$$ | $$\begin{align*}&43\\&55\\&57\\&57\end{align*}$$ | $$\begin{align*}&40\\&49\\&50\\&50\end{align*}$$ |
| $$\begin{align*}&38\\&44\\&45\\&45\end{align*}$$ | $$\begin{align*}&36\\&41\\&42\\&42\end{align*}$$ | $$\begin{align*}&35\\&38\\&39\\&39\end{align*}$$ | $$\begin{align*}&33\\&36\\&36\\&36\end{align*}$$ | $$\begin{align*}&31\\&31\\&31\\&31\end{align*}$$ |

#### Case 3 Formula 3

<iframe width="1000" height="800" src="https://www.youtube.com/embed/An3KCNPbg-U" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

| $$\begin{align*}&949\\&1799\\&1799\\&1799\end{align*}$$ | $$\begin{align*}&568\\&1178\\&1257\\&1280\end{align*}$$ | $$\begin{align*}&341\\&644\\&681\\&703\end{align*}$$ | $$\begin{align*}&256\\&457\\&483\\&494\end{align*}$$ | $$\begin{align*}&210\\&360\\&379\\&388\end{align*}$$ |
| $$\begin{align*}&182\\&301\\&316\\&323\end{align*}$$ | $$\begin{align*}&162\\&261\\&273\\&279\end{align*}$$ | $$\begin{align*}&148\\&232\\&243\\&248\end{align*}$$ | $$\begin{align*}&137\\&210\\&220\\&224\end{align*}$$ | $$\begin{align*}&128\\&193\\&201\\&205\end{align*}$$ |
| $$\begin{align*}&121\\&180\\&187\\&190\end{align*}$$ | $$\begin{align*}&89\\&117\\&120\\&122\end{align*}$$ | $$\begin{align*}&79\\&96\\&98\\&99\end{align*}$$ | $$\begin{align*}&73\\&85\\&87\\&87\end{align*}$$ | $$\begin{align*}&70\\&79\\&80\\&80\end{align*}$$ |
| $$\begin{align*}&68\\&74\\&75\\&75\end{align*}$$ | $$\begin{align*}&66\\&71\\&72\\&72\end{align*}$$ | $$\begin{align*}&65\\&68\\&69\\&69\end{align*}$$ | $$\begin{align*}&63\\&66\\&66\\&66\end{align*}$$ | $$\begin{align*}&61\\&61\\&61\\&61\end{align*}$$ |

#### Case 4 Formula 3

<video width="1000" height="800" controls>
  <source src="{{site.url}}/videos/case_4_form_3.mp4" type="video/mp4">
Your browser does not support the video tag.
</video>

| $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ |
| $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ |
| $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ |
| $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ |

#### Remarks on Formula 3

#### Case 1 Formula 4

<iframe width="1000" height="800" src="https://www.youtube.com/embed/4QP8NNYn7JA" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

| $$\begin{align*}&1320\\&1799\\&1799\\&1799\end{align*}$$ | $$\begin{align*}&674\\&1217\\&1284\\&1294\end{align*}$$ | $$\begin{align*}&350\\&622\\&651\\&656\end{align*}$$ | $$\begin{align*}&243\\&430\\&458\\&467\end{align*}$$ | $$\begin{align*}&189\\&332\\&359\\&369\end{align*}$$ |
| $$\begin{align*}&157\\&269\\&287\\&292\end{align*}$$ | $$\begin{align*}&135\\&227\\&241\\&245\end{align*}$$ | $$\begin{align*}&120\\&199\\&211\\&214\end{align*}$$ | $$\begin{align*}&108\\&177\\&188\\&190\end{align*}$$ | $$\begin{align*}&99\\&161\\&171\\&174\end{align*}$$ |
| $$\begin{align*}&92\\&147\\&155\\&158\end{align*}$$ | $$\begin{align*}&59\\&85\\&89\\&90\end{align*}$$ | $$\begin{align*}&49\\&65\\&67\\&68\end{align*}$$ | $$\begin{align*}&43\\&54\\&56\\&57\end{align*}$$ | $$\begin{align*}&40\\&48\\&49\\&50\end{align*}$$ |
| $$\begin{align*}&38\\&44\\&45\\&45\end{align*}$$ | $$\begin{align*}&36\\&41\\&41\\&42\end{align*}$$ | $$\begin{align*}&35\\&38\\&39\\&39\end{align*}$$ | $$\begin{align*}&33\\&36\\&36\\&36\end{align*}$$ | $$\begin{align*}&31\\&31\\&31\\&31\end{align*}$$ |

#### Case 2 Formula 4

<iframe width="1000" height="800" src="https://www.youtube.com/embed/FcHARWRKvtg" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

| $$\begin{align*}&1320\\&1799\\&1799\\&1799\end{align*}$$ | $$\begin{align*}&673\\&1277\\&1350\\&1372\end{align*}$$ | $$\begin{align*}&350\\&650\\&684\\&699\end{align*}$$ | $$\begin{align*}&243\\&441\\&464\\&474\end{align*}$$ | $$\begin{align*}&189\\&340\\&360\\&370\end{align*}$$ |
| $$\begin{align*}&156\\&276\\&292\\&299\end{align*}$$ | $$\begin{align*}&135\\&234\\&246\\&252\end{align*}$$ | $$\begin{align*}&120\\&204\\&214\\&219\end{align*}$$ | $$\begin{align*}&108\\&181\\&190\\&194\end{align*}$$ | $$\begin{align*}&99\\&164\\&173\\&177\end{align*}$$ |
| $$\begin{align*}&92\\&150\\&158\\&161\end{align*}$$ | $$\begin{align*}&59\\&87\\&90\\&92\end{align*}$$ | $$\begin{align*}&49\\&66\\&68\\&69\end{align*}$$ | $$\begin{align*}&43\\&55\\&57\\&57\end{align*}$$ | $$\begin{align*}&40\\&49\\&50\\&50\end{align*}$$ |
| $$\begin{align*}&38\\&44\\&45\\&45\end{align*}$$ | $$\begin{align*}&36\\&41\\&42\\&42\end{align*}$$ | $$\begin{align*}&35\\&38\\&39\\&39\end{align*}$$ | $$\begin{align*}&33\\&36\\&36\\&36\end{align*}$$ | $$\begin{align*}&31\\&31\\&31\\&31\end{align*}$$ |


#### Case 3 Formula 4

<iframe width="1000" height="800" src="https://www.youtube.com/embed/9m0it860VzY" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

| $$\begin{align*}&949\\&1799\\&1799\\&1799\end{align*}$$ | $$\begin{align*}&568\\&1171\\&1244\\&1266\end{align*}$$ | $$\begin{align*}&341\\&640\\&675\\&690\end{align*}$$ | $$\begin{align*}&256\\&454\\&477\\&487\end{align*}$$ | $$\begin{align*}&210\\&362\\&382\\&392\end{align*}$$ |
| $$\begin{align*}&182\\&302\\&317\\&324\end{align*}$$ | $$\begin{align*}&162\\&261\\&274\\&279\end{align*}$$ | $$\begin{align*}&148\\&232\\&242\\&247\end{align*}$$ | $$\begin{align*}&137\\&210\\&219\\&223\end{align*}$$ | $$\begin{align*}&128\\&194\\&202\\&206\end{align*}$$ |
| $$\begin{align*}&121\\&180\\&187\\&191\end{align*}$$ | $$\begin{align*}&89\\&117\\&120\\&122\end{align*}$$ | $$\begin{align*}&79\\&96\\&98\\&99\end{align*}$$ | $$\begin{align*}&73\\&85\\&87\\&87\end{align*}$$ | $$\begin{align*}&70\\&79\\&80\\&80\end{align*}$$ |
| $$\begin{align*}&68\\&74\\&75\\&75\end{align*}$$ | $$\begin{align*}&66\\&71\\&72\\&72\end{align*}$$ | $$\begin{align*}&65\\&68\\&69\\&69\end{align*}$$ | $$\begin{align*}&63\\&66\\&66\\&66\end{align*}$$ | $$\begin{align*}&61\\&61\\&61\\&61\end{align*}$$ |

#### Case 4 Formula 4

<video width="1000" height="800" controls>
  <source src="{{site.url}}/videos/case_4_form_4.mp4" type="video/mp4">
Your browser does not support the video tag.
</video>

| $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ |
| $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ |
| $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ |
| $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ | $$\begin{align*}&600*600\\&600\\&60\\&0\end{align*}$$ |

#### Remarks on Formula 4

## Final Remarks
