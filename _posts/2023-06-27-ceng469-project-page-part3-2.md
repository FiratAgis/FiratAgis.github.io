# Ceng469 Final Project: Temporal Tone Mapping for Simulating Human Visual System: Part 3.2: Formula 2

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

### Formula 2

$$\begin{align*}
\overline{L}_w(i) &= \exp{\left(\dfrac{\sum_{x, y} \log{(\epsilon + L_w(x,y,i))}}{N} \right)}\\
\dot{L}_w(i) &= \begin{cases}\ \overline{L}_w(i)&, i=0\\ \text{av}_2(w, \overline{L}_w(i), \dot{L}_w(i-1))&, i \neq 0  \end{cases}\\
\text{av}_2(w, \overline{L}_w, \dot{L}_w) &= w * \overline{L}_w + (1-w)\dot{L}_w\\
\text{Rend}&\left(\frac{\alpha}{\dot{L}_w(i)}\right)
\end{align*}$$

#### Scenerio 1

<iframe width="1000" height="800" src="https://www.youtube.com/embed/ySNwRwLw_es" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

| $$\begin{align*}&975\\&1799\\&1799\\&1799\end{align*}$$ | $$\begin{align*}&502\\&1070\\&1154\\&1179\end{align*}$$ | $$\begin{align*}&265\\&548\\&590\\&599\end{align*}$$ | $$\begin{align*}&186\\&374\\&402\\&408\end{align*}$$ | $$\begin{align*}&146\\&288\\&311\\&316\end{align*}$$ |
| $$\begin{align*}&123\\&235\\&253\\&257\end{align*}$$ | $$\begin{align*}&107\\&200\\&215\\&218\end{align*}$$ | $$\begin{align*}&96\\&175\\&187\\&190\end{align*}$$ | $$\begin{align*}&87\\&156\\&167\\&169\end{align*}$$ | $$\begin{align*}&81\\&142\\&152\\&154\end{align*}$$ |
| $$\begin{align*}&75\\&130\\&139\\&141\end{align*}$$ | $$\begin{align*}&52\\&77\\&81\\&82\end{align*}$$ | $$\begin{align*}&44\\&60\\&62\\&63\end{align*}$$ | $$\begin{align*}&40\\&51\\&53\\&53\end{align*}$$ | $$\begin{align*}&37\\&46\\&47\\&47\end{align*}$$ |
| $$\begin{align*}&36\\&42\\&43\\&43\end{align*}$$ | $$\begin{align*}&34\\&39\\&40\\&40\end{align*}$$ | $$\begin{align*}&33\\&37\\&38\\&38\end{align*}$$ | $$\begin{align*}&33\\&35\\&35\\&36\end{align*}$$ | $$\begin{align*}&31\\&31\\&31\\&31\end{align*}$$ |

#### Scenerio 2

<iframe width="1000" height="800" src="https://www.youtube.com/embed/5JUWPGsopaI" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

| $$\begin{align*}&1799\\&1799\\&1799\\&1799\end{align*}$$ | $$\begin{align*}&1060\\&1690\\&1790\\&1799\end{align*}$$ | $$\begin{align*}&542\\&843\\&878\\&894\end{align*}$$ | $$\begin{align*}&370\\&572\\&598\\&611\end{align*}$$ | $$\begin{align*}&284\\&434\\&452\\&460\end{align*}$$ |
| $$\begin{align*}&232\\&352\\&367\\&375\end{align*}$$ | $$\begin{align*}&198\\&296\\&308\\&313\end{align*}$$ | $$\begin{align*}&173\\&257\\&268\\&273\end{align*}$$ | $$\begin{align*}&155\\&228\\&238\\&242\end{align*}$$ | $$\begin{align*}&140\\&205\\&213\\&217\end{align*}$$ |
| $$\begin{align*}&129\\&187\\&194\\&197\end{align*}$$ | $$\begin{align*}&77\\&104\\&108\\&109\end{align*}$$ | $$\begin{align*}&59\\&77\\&79\\&80\end{align*}$$ | $$\begin{align*}&51\\&63\\&64\\&65\end{align*}$$ | $$\begin{align*}&45\\&54\\&55\\&56\end{align*}$$ |
| $$\begin{align*}&42\\&48\\&49\\&50\end{align*}$$ | $$\begin{align*}&39\\&44\\&45\\&45\end{align*}$$ | $$\begin{align*}&37\\&41\\&41\\&41\end{align*}$$ | $$\begin{align*}&35\\&38\\&38\\&38\end{align*}$$ | $$\begin{align*}&31\\&31\\&31\\&31\end{align*}$$ |

#### Scenerio 3

<iframe src="https://drive.google.com/file/d/12mIWfiYW0-t4-W3jmgF4fnNo6CW5YNvM/preview" width="1000" height="800" allow="autoplay"></iframe>

| $$\begin{align*}&1731\\&1799\\&1799\\&1799\end{align*}$$ | $$\begin{align*}&956\\&1586\\&1686\\&1799\end{align*}$$ | $$\begin{align*}&533\\&834\\&869\\&885\end{align*}$$ | $$\begin{align*}&383\\&585\\&611\\&624\end{align*}$$ | $$\begin{align*}&305\\&455\\&473\\&481\end{align*}$$ |
| $$\begin{align*}&257\\&377\\&392\\&400\end{align*}$$ | $$\begin{align*}&225\\&324\\&335\\&340\end{align*}$$ | $$\begin{align*}&201\\&286\\&296\\&301\end{align*}$$ | $$\begin{align*}&184\\&257\\&267\\&271\end{align*}$$ | $$\begin{align*}&170\\&235\\&243\\&246\end{align*}$$ |
| $$\begin{align*}&158\\&216\\&223\\&226\end{align*}$$ | $$\begin{align*}&107\\&134\\&138\\&139\end{align*}$$ | $$\begin{align*}&89\\&107\\&109\\&110\end{align*}$$ | $$\begin{align*}&81\\&93\\&94\\&95\end{align*}$$ | $$\begin{align*}&75\\&84\\&85\\&86\end{align*}$$ |
| $$\begin{align*}&72\\&78\\&79\\&80\end{align*}$$ | $$\begin{align*}&69\\&74\\&75\\&75\end{align*}$$ | $$\begin{align*}&67\\&71\\&71\\&71\end{align*}$$ | $$\begin{align*}&65\\&68\\&68\\&68\end{align*}$$ | $$\begin{align*}&61\\&61\\&61\\&61\end{align*}$$ |

#### Scenerio 4

<iframe src="https://drive.google.com/file/d/17lsx35wzyYCIqH468Y02xlSjIkXK6Uae/preview" width="1000" height="800" allow="autoplay"></iframe>

| $$\begin{align*}&612\\&1705\\&1799\\&1799\end{align*}$$ | $$\begin{align*}&398\\&942\\&1002\\&1096\end{align*}$$ | $$\begin{align*}&256\\&532\\&567\\&634\end{align*}$$ | $$\begin{align*}&199\\&381\\&403\\&440\end{align*}$$ | $$\begin{align*}&168\\&306\\&324\\&363\end{align*}$$ |
| $$\begin{align*}&148\\&257\\&271\\&298\end{align*}$$ | $$\begin{align*}&134\\&225\\&236\\&258\end{align*}$$ | $$\begin{align*}&124\\&201\\&211\\&229\end{align*}$$ | $$\begin{align*}&116\\&183\\&192\\&207\end{align*}$$ | $$\begin{align*}&110\\&170\\&177\\&193\end{align*}$$ |
| $$\begin{align*}&105\\&158\\&165\\&178\end{align*}$$ | $$\begin{align*}&82\\&107\\&110\\&116\end{align*}$$ | $$\begin{align*}&74\\&89\\&91\\&95\end{align*}$$ | $$\begin{align*}&70\\&81\\&82\\&85\end{align*}$$ | $$\begin{align*}&67\\&75\\&76\\&78\end{align*}$$ |
| $$\begin{align*}&66\\&72\\&73\\&74\end{align*}$$ | $$\begin{align*}&64\\&69\\&70\\&71\end{align*}$$ | $$\begin{align*}&63\\&67\\&67\\&68\end{align*}$$ | $$\begin{align*}&63\\&65\\&65\\&66\end{align*}$$ | $$\begin{align*}&61\\&61\\&61\\&61\end{align*}$$ |

#### Remarks on Formula 2

While looking at the results of Formula 2 in isolation (see <a href="{{site.url}}/2023/06/27/ceng469-project-page-part3-5">Part 3.5: Comparing Results</a> for the big picture), two major phenomenas are significant in my opinion. Even if I am looking Formula 2 in isolotion, I won't be repeating any point I made during my discussion of Formula 1, instead I will reference them

Firstly, the effects of light increaases and decreases are asymetric (just like formula 1), but in this case it takes more time to get use to decreases in the light level instead of increases in the light level. While this asymetry is generally undesirable as I discussed in the remarks of Formula 1, it has some uses. While scenerio 1 becomes very in efective, this simulates how everything becomes hard to see after a flash of light during the scenerio 3 very well.

Secondly, the issue of not being able to similutate afterimages created by sudden flashes persists, but because the methodology lacks a way of differentiating flashes from other changes in light, simple changes in the formula would not be able to to change this fact. 
