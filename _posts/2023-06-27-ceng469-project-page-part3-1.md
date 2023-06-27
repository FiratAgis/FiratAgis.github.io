# Ceng469 Final Project: Temporal Tone Mapping for Simulating Human Visual System: Part 3.1: Formula 1

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

### Formula 1

$$\begin{align*}
\overline{L}_w(i) &= \dfrac{\alpha}{\exp{\left(\dfrac{\sum_{x, y} \log{(\epsilon + L_w(x,y,i))}}{N} \right)}}\\
\dot{L}_w(i) &= \begin{cases}\ \overline{L}_w(i)&, i=0\\ \text{av}_1(w, \overline{L}_w(i), \dot{L}_w(i-1))&, i \neq 0  \end{cases}\\
\text{av}_1(w, \overline{L}_w, \dot{L}_w) &= w * \overline{L}_w + (1-w)\dot{L}_w\\
\text{Rend}&(\dot{L}_w(i))
\end{align*}$$

#### Scenerio 1

<iframe width="1000" height="800" src="https://www.youtube.com/embed/0su4lINw-Ig" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

| $$\begin{align*}&1799\\&1799\\&1799\\&1799\end{align*}$$ | $$\begin{align*}&1063\\&1643\\&1744\\&1779\end{align*}$$ | $$\begin{align*}&544\\&833\\&884\\&897\end{align*}$$ | $$\begin{align*}&371\\&560\\&590\\&597\end{align*}$$ | $$\begin{align*}&285\\&425\\&446\\&450\end{align*}$$ |
| $$\begin{align*}&233\\&345\\&362\\&366\end{align*}$$ | $$\begin{align*}&198\\&291\\&306\\&309\end{align*}$$ | $$\begin{align*}&173\\&253\\&266\\&269\end{align*}$$ | $$\begin{align*}&155\\&224\\&235\\&237\end{align*}$$ | $$\begin{align*}&140\\&201\\&211\\&213\end{align*}$$ |
| $$\begin{align*}&129\\&183\\&192\\&194\end{align*}$$ | $$\begin{align*}&77\\&103\\&107\\&108\end{align*}$$ | $$\begin{align*}&60\\&76\\&78\\&79\end{align*}$$ | $$\begin{align*}&51\\&62\\&64\\&64\end{align*}$$ | $$\begin{align*}&45\\&54\\&55\\&55\end{align*}$$ |
| $$\begin{align*}&42\\&48\\&49\\&49\end{align*}$$ | $$\begin{align*}&39\\&44\\&45\\&45\end{align*}$$ | $$\begin{align*}&37\\&41\\&41\\&41\end{align*}$$ | $$\begin{align*}&35\\&38\\&38\\&38\end{align*}$$ | $$\begin{align*}&31\\&31\\&31\\&31\end{align*}$$ |

#### Scenerio 2

<iframe width="1000" height="800" src="https://www.youtube.com/embed/mDigGMG7B7o" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

| $$\begin{align*}&976\\&1799\\&1799\\&1799\end{align*}$$ | $$\begin{align*}&502\\&1108\\&1184\\&1207\end{align*}$$ | $$\begin{align*}&265\\&566\\&602\\&620\end{align*}$$ | $$\begin{align*}&186\\&384\\&406\\&416\end{align*}$$ | $$\begin{align*}&147\\&297\\&315\\&324\end{align*}$$ |
| $$\begin{align*}&123\\&242\\&257\\&263\end{align*}$$ | $$\begin{align*}&107\\&206\\&218\\&223\end{align*}$$ | $$\begin{align*}&96\\&180\\&190\\&195\end{align*}$$ | $$\begin{align*}&87\\&160\\&169\\&173\end{align*}$$ | $$\begin{align*}&81\\&146\\&154\\&158\end{align*}$$ |
| $$\begin{align*}&75\\&134\\&141\\&144\end{align*}$$ | $$\begin{align*}&52\\&79\\&83\\&84\end{align*}$$ | $$\begin{align*}&44\\&61\\&63\\&64\end{align*}$$ | $$\begin{align*}&40\\&52\\&53\\&54\end{align*}$$ | $$\begin{align*}&37\\&46\\&47\\&48\end{align*}$$ |
| $$\begin{align*}&36\\&42\\&43\\&44\end{align*}$$ | $$\begin{align*}&34\\&40\\&40\\&40\end{align*}$$ | $$\begin{align*}&33\\&37\\&38\\&38\end{align*}$$ | $$\begin{align*}&33\\&35\\&36\\&36\end{align*}$$ | $$\begin{align*}&31\\&31\\&31\\&31\end{align*}$$ |

#### Scenerio 3

<iframe src="https://drive.google.com/file/d/1YxCeWQUTlOwWKLc25QV2QlCLMOQrHHLH/preview" width="1000" height="800" allow="autoplay"></iframe>

| $$\begin{align*}&564\\&1690\\&1777\\&1799\end{align*}$$ | $$\begin{align*}&387\\&993\\&1069\\&1092\end{align*}$$ | $$\begin{align*}&254\\&554\\&590\\&608\end{align*}$$ | $$\begin{align*}&198\\&369\\&419\\&428\end{align*}$$ | $$\begin{align*}&168\\&318\\&336\\&345\end{align*}$$ |
| $$\begin{align*}&148\\&267\\&282\\&289\end{align*}$$ | $$\begin{align*}&134\\&233\\&245\\&250\end{align*}$$ | $$\begin{align*}&124\\&208\\&218\\&223\end{align*}$$ | $$\begin{align*}&116\\&289\\&198\\&202\end{align*}$$ | $$\begin{align*}&110\\&175\\&183\\&187\end{align*}$$ |
| $$\begin{align*}&105\\&163\\&171\\&174\end{align*}$$ | $$\begin{align*}&82\\&109\\&113\\&114\end{align*}$$ | $$\begin{align*}&74\\&91\\&93\\&94\end{align*}$$ | $$\begin{align*}&70\\&82\\&83\\&84\end{align*}$$ | $$\begin{align*}&67\\&76\\&77\\&78\end{align*}$$ |
| $$\begin{align*}&62\\&72\\&73\\&74\end{align*}$$ | $$\begin{align*}&64\\&70\\&70\\&70\end{align*}$$ | $$\begin{align*}&63\\&67\\&68\\&68\end{align*}$$ | $$\begin{align*}&63\\&65\\&66\\&66\end{align*}$$ | $$\begin{align*}&61\\&61\\&61\\&61\end{align*}$$ |

#### Scenerio 4

<iframe src="https://drive.google.com/file/d/1VhuaCmGalR4tbXZ4gzXLHLZW61LYEwNe/preview" width="1000" height="800" allow="autoplay"></iframe>

| $$\begin{align*}&1730\\&1799\\&1799\\&1799\end{align*}$$ | $$\begin{align*}&955\\&1536\\&1637\\&1672\end{align*}$$ | $$\begin{align*}&533\\&822\\&873\\&886\end{align*}$$ | $$\begin{align*}&383\\&572\\&602\\&609\end{align*}$$ | $$\begin{align*}&305\\&445\\&466\\&471\end{align*}$$ |
| $$\begin{align*}&257\\&369\\&387\\&391\end{align*}$$ | $$\begin{align*}&225\\&318\\&333\\&336\end{align*}$$ | $$\begin{align*}&201\\&281\\&294\\&297\end{align*}$$ | $$\begin{align*}&183\\&252\\&263\\&266\end{align*}$$ | $$\begin{align*}&169\\&230\\&239\\&242\end{align*}$$ |
| $$\begin{align*}&158\\&213\\&221\\&223\end{align*}$$ | $$\begin{align*}&107\\&133\\&137\\&138\end{align*}$$ | $$\begin{align*}&89\\&106\\&108\\&109\end{align*}$$ | $$\begin{align*}&81\\&92\\&94\\&94\end{align*}$$ | $$\begin{align*}&75\\&84\\&85\\&85\end{align*}$$ |
| $$\begin{align*}&72\\&78\\&79\\&79\end{align*}$$ | $$\begin{align*}&69\\&74\\&75\\&75\end{align*}$$ | $$\begin{align*}&67\\&70\\&71\\&71\end{align*}$$ | $$\begin{align*}&65\\&67\\&68\\&68\end{align*}$$ | $$\begin{align*}&61\\&61\\&61\\&61\end{align*}$$ |

#### Remarks on Formula 1

While looking at the results of Formula 1 in isolation (see <a href="{{site.url}}/2023/06/27/ceng469-project-page-part3-5">Part 3.5: Comparing Results</a> for the big picture), two major phenomenas are significant in my opinion. 

Firstly, the convergence speed is asymetric. Meaning it converges slowly when getting used to increases in light, but rapidly in the case of decreasing light. I suspect this is the result of arithmetic averaging while using logarithm based values. If this formula is choosen for future implementation, I suggest the usage different weight for light increases and decreases to compansate for this, but especially in the case of dynamically choosen weights, this property of formula 1 makes it undesirable.

Secondly, some what because of the first phenomena, flashes given in the scenerios 3 and 4 look unideal. In scenerio 3, it barely gets used to increase in the light level before rapidly returning to its original state. In scenerio 4, the avarage white  plunges very quickly before the light level returns to its initial state, making the scene very similar to scenerio 1, only with a slight delay. But also, the formula fails to simulate how a sudden increase in light creates an afterimage, making everything to look bright for a brief amount of time, even after the light level returns to normal. But this formula (and this method in general) makse no differentiation wbetween a sudden flash that bunrs the observers eyes and a well-lit environment that the observer is already used to.
