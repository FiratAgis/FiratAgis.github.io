# Ceng469 Final Project: Temporal Tone Mapping for Simulating Human Visual System: Part3.3: Formula 3

My project aims to model how the Human Visual System (HVS) adjusts to cahnges in light level. I plan to achieve this by using temporal tonemapping operators. To be more spesific, I will be modifying Reinhard's Global Operator as a rolling average between frames, both as a aritmatic and geometric average to compare the results.

## Navigation

## Results

### Formula 3

#### Scenerio 1

<iframe width="1000" height="800" src="https://www.youtube.com/embed/WKaipqUgVxo" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

| $$\begin{align*}&1321\\&1799\\&1799\\&1799\end{align*}$$ | $$\begin{align*}&674\\&1212\\&1247\\&1799\end{align*}$$ | $$\begin{align*}&350\\&616\\&644\\&648\end{align*}$$ | $$\begin{align*}&243\\&434\\&467\\&1799\end{align*}$$ | $$\begin{align*}&189\\&336\\&1799\\&1799\end{align*}$$ |
| $$\begin{align*}&157\\&271\\&292\\&296\end{align*}$$ | $$\begin{align*}&135\\&228\\&242\\&246\end{align*}$$ | $$\begin{align*}&120\\&198\\&209\\&211\end{align*}$$ | $$\begin{align*}&108\\&176\\&184\\&186\end{align*}$$ | $$\begin{align*}&99\\&162\\&174\\&178\end{align*}$$ |
| $$\begin{align*}&92\\&147\\&157\\&159\end{align*}$$ | $$\begin{align*}&59\\&85\\&89\\&90\end{align*}$$ | $$\begin{align*}&49\\&65\\&67\\&68\end{align*}$$ | $$\begin{align*}&43\\&54\\&56\\&56\end{align*}$$ | $$\begin{align*}&40\\&48\\&49\\&50\end{align*}$$ |
| $$\begin{align*}&38\\&44\\&45\\&45\end{align*}$$ | $$\begin{align*}&36\\&41\\&41\\&42\end{align*}$$ | $$\begin{align*}&35\\&38\\&39\\&39\end{align*}$$ | $$\begin{align*}&33\\&36\\&36\\&36\end{align*}$$ | $$\begin{align*}&31\\&31\\&31\\&31\end{align*}$$ |

#### Scenerio 2

<iframe width="1000" height="800" src="https://www.youtube.com/embed/nDYNq5cXMRQ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

| $$\begin{align*}&1320\\&1799\\&1799\\&1799\end{align*}$$ | $$\begin{align*}&674\\&1284\\&1363\\&1386\end{align*}$$ | $$\begin{align*}&350\\&654\\&690\\&712\end{align*}$$ | $$\begin{align*}&243\\&444\\&469\\&481\end{align*}$$ | $$\begin{align*}&189\\&339\\&358\\&367\end{align*}$$ |
| $$\begin{align*}&156\\&276\\&291\\&298\end{align*}$$ | $$\begin{align*}&135\\&234\\&246\\&252\end{align*}$$ | $$\begin{align*}&120\\&204\\&214\\&219\end{align*}$$ | $$\begin{align*}&108\\&182\\&191\\&195\end{align*}$$ | $$\begin{align*}&99\\&164\\&172\\&176\end{align*}$$ |
| $$\begin{align*}&92\\&150\\&157\\&161\end{align*}$$ | $$\begin{align*}&59\\&87\\&90\\&92\end{align*}$$ | $$\begin{align*}&49\\&66\\&68\\&69\end{align*}$$ | $$\begin{align*}&43\\&55\\&57\\&57\end{align*}$$ | $$\begin{align*}&40\\&49\\&50\\&50\end{align*}$$ |
| $$\begin{align*}&38\\&44\\&45\\&45\end{align*}$$ | $$\begin{align*}&36\\&41\\&42\\&42\end{align*}$$ | $$\begin{align*}&35\\&38\\&39\\&39\end{align*}$$ | $$\begin{align*}&33\\&36\\&36\\&36\end{align*}$$ | $$\begin{align*}&31\\&31\\&31\\&31\end{align*}$$ |

#### Scenerio 3

<iframe width="1000" height="800" src="https://www.youtube.com/embed/An3KCNPbg-U" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

| $$\begin{align*}&949\\&1799\\&1799\\&1799\end{align*}$$ | $$\begin{align*}&568\\&1178\\&1257\\&1280\end{align*}$$ | $$\begin{align*}&341\\&644\\&681\\&703\end{align*}$$ | $$\begin{align*}&256\\&457\\&483\\&494\end{align*}$$ | $$\begin{align*}&210\\&360\\&379\\&388\end{align*}$$ |
| $$\begin{align*}&182\\&301\\&316\\&323\end{align*}$$ | $$\begin{align*}&162\\&261\\&273\\&279\end{align*}$$ | $$\begin{align*}&148\\&232\\&243\\&248\end{align*}$$ | $$\begin{align*}&137\\&210\\&220\\&224\end{align*}$$ | $$\begin{align*}&128\\&193\\&201\\&205\end{align*}$$ |
| $$\begin{align*}&121\\&180\\&187\\&190\end{align*}$$ | $$\begin{align*}&89\\&117\\&120\\&122\end{align*}$$ | $$\begin{align*}&79\\&96\\&98\\&99\end{align*}$$ | $$\begin{align*}&73\\&85\\&87\\&87\end{align*}$$ | $$\begin{align*}&70\\&79\\&80\\&80\end{align*}$$ |
| $$\begin{align*}&68\\&74\\&75\\&75\end{align*}$$ | $$\begin{align*}&66\\&71\\&72\\&72\end{align*}$$ | $$\begin{align*}&65\\&68\\&69\\&69\end{align*}$$ | $$\begin{align*}&63\\&66\\&66\\&66\end{align*}$$ | $$\begin{align*}&61\\&61\\&61\\&61\end{align*}$$ |

#### Scenerio 4

<iframe src="https://drive.google.com/file/d/18Za0YIuRGz1LenW_tWE4RBDWxyB4Dt5X/preview" width="1000" height="800" allow="autoplay"></iframe>

| $$\begin{align*}&957\\&1799\\&1799\\&1799\end{align*}$$ | $$\begin{align*}&569\\&1108\\&1143\\&1799\end{align*}$$ | $$\begin{align*}&341\\&606\\&634\\&658\end{align*}$$ | $$\begin{align*}&256\\&431\\&448\\&470\end{align*}$$ | $$\begin{align*}&210\\&347\\&363\\&1799\end{align*}$$ |
| $$\begin{align*}&182\\&289\\&302\\&321\end{align*}$$ | $$\begin{align*}&162\\&253\\&266\\&1799\end{align*}$$ | $$\begin{align*}&148\\&226\\&237\\&1799\end{align*}$$ | $$\begin{align*}&137\\&205\\&213\\&230\end{align*}$$ | $$\begin{align*}&128\\&189\\&198\\&1799\end{align*}$$ |
| $$\begin{align*}&121\\&175\\&183\\&198\end{align*}$$ | $$\begin{align*}&89\\&114\\&118\\&123\end{align*}$$ | $$\begin{align*}&79\\&94\\&96\\&100\end{align*}$$ | $$\begin{align*}&73\\&84\\&85\\&88\end{align*}$$ | $$\begin{align*}&70\\&78\\&79\\&81\end{align*}$$ |
| $$\begin{align*}&68\\&74\\&74\\&76\end{align*}$$ | $$\begin{align*}&66\\&71\\&71\\&72\end{align*}$$ | $$\begin{align*}&65\\&68\\&68\\&69\end{align*}$$ | $$\begin{align*}&63\\&66\\&66\\&67\end{align*}$$ | $$\begin{align*}&61\\&61\\&61\\&61\end{align*}$$ |

#### Remarks on Formula 3
