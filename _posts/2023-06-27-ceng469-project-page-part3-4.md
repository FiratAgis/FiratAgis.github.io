# Ceng469 Final Project: Temporal Tone Mapping for Simulating Human Visual System: Part3.1: Formula 1

My project aims to model how the Human Visual System (HVS) adjusts to cahnges in light level. I plan to achieve this by using temporal tonemapping operators. To be more spesific, I will be modifying Reinhard's Global Operator as a rolling average between frames, both as a aritmatic and geometric average to compare the results.

## Navigation

## Results

### Formula 4

#### Scenerio 1

<iframe width="1000" height="800" src="https://www.youtube.com/embed/4QP8NNYn7JA" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

| $$\begin{align*}&1320\\&1799\\&1799\\&1799\end{align*}$$ | $$\begin{align*}&674\\&1217\\&1284\\&1294\end{align*}$$ | $$\begin{align*}&350\\&622\\&651\\&656\end{align*}$$ | $$\begin{align*}&243\\&430\\&458\\&467\end{align*}$$ | $$\begin{align*}&189\\&332\\&359\\&369\end{align*}$$ |
| $$\begin{align*}&157\\&269\\&287\\&292\end{align*}$$ | $$\begin{align*}&135\\&227\\&241\\&245\end{align*}$$ | $$\begin{align*}&120\\&199\\&211\\&214\end{align*}$$ | $$\begin{align*}&108\\&177\\&188\\&190\end{align*}$$ | $$\begin{align*}&99\\&161\\&171\\&174\end{align*}$$ |
| $$\begin{align*}&92\\&147\\&155\\&158\end{align*}$$ | $$\begin{align*}&59\\&85\\&89\\&90\end{align*}$$ | $$\begin{align*}&49\\&65\\&67\\&68\end{align*}$$ | $$\begin{align*}&43\\&54\\&56\\&57\end{align*}$$ | $$\begin{align*}&40\\&48\\&49\\&50\end{align*}$$ |
| $$\begin{align*}&38\\&44\\&45\\&45\end{align*}$$ | $$\begin{align*}&36\\&41\\&41\\&42\end{align*}$$ | $$\begin{align*}&35\\&38\\&39\\&39\end{align*}$$ | $$\begin{align*}&33\\&36\\&36\\&36\end{align*}$$ | $$\begin{align*}&31\\&31\\&31\\&31\end{align*}$$ |

#### Scenerio 2

<iframe width="1000" height="800" src="https://www.youtube.com/embed/FcHARWRKvtg" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

| $$\begin{align*}&1320\\&1799\\&1799\\&1799\end{align*}$$ | $$\begin{align*}&673\\&1277\\&1350\\&1372\end{align*}$$ | $$\begin{align*}&350\\&650\\&684\\&699\end{align*}$$ | $$\begin{align*}&243\\&441\\&464\\&474\end{align*}$$ | $$\begin{align*}&189\\&340\\&360\\&370\end{align*}$$ |
| $$\begin{align*}&156\\&276\\&292\\&299\end{align*}$$ | $$\begin{align*}&135\\&234\\&246\\&252\end{align*}$$ | $$\begin{align*}&120\\&204\\&214\\&219\end{align*}$$ | $$\begin{align*}&108\\&181\\&190\\&194\end{align*}$$ | $$\begin{align*}&99\\&164\\&173\\&177\end{align*}$$ |
| $$\begin{align*}&92\\&150\\&158\\&161\end{align*}$$ | $$\begin{align*}&59\\&87\\&90\\&92\end{align*}$$ | $$\begin{align*}&49\\&66\\&68\\&69\end{align*}$$ | $$\begin{align*}&43\\&55\\&57\\&57\end{align*}$$ | $$\begin{align*}&40\\&49\\&50\\&50\end{align*}$$ |
| $$\begin{align*}&38\\&44\\&45\\&45\end{align*}$$ | $$\begin{align*}&36\\&41\\&42\\&42\end{align*}$$ | $$\begin{align*}&35\\&38\\&39\\&39\end{align*}$$ | $$\begin{align*}&33\\&36\\&36\\&36\end{align*}$$ | $$\begin{align*}&31\\&31\\&31\\&31\end{align*}$$ |


#### Scenerio 3

<iframe width="1000" height="800" src="https://www.youtube.com/embed/9m0it860VzY" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

| $$\begin{align*}&949\\&1799\\&1799\\&1799\end{align*}$$ | $$\begin{align*}&568\\&1171\\&1244\\&1266\end{align*}$$ | $$\begin{align*}&341\\&640\\&675\\&690\end{align*}$$ | $$\begin{align*}&256\\&454\\&477\\&487\end{align*}$$ | $$\begin{align*}&210\\&362\\&382\\&392\end{align*}$$ |
| $$\begin{align*}&182\\&302\\&317\\&324\end{align*}$$ | $$\begin{align*}&162\\&261\\&274\\&279\end{align*}$$ | $$\begin{align*}&148\\&232\\&242\\&247\end{align*}$$ | $$\begin{align*}&137\\&210\\&219\\&223\end{align*}$$ | $$\begin{align*}&128\\&194\\&202\\&206\end{align*}$$ |
| $$\begin{align*}&121\\&180\\&187\\&191\end{align*}$$ | $$\begin{align*}&89\\&117\\&120\\&122\end{align*}$$ | $$\begin{align*}&79\\&96\\&98\\&99\end{align*}$$ | $$\begin{align*}&73\\&85\\&87\\&87\end{align*}$$ | $$\begin{align*}&70\\&79\\&80\\&80\end{align*}$$ |
| $$\begin{align*}&68\\&74\\&75\\&75\end{align*}$$ | $$\begin{align*}&66\\&71\\&72\\&72\end{align*}$$ | $$\begin{align*}&65\\&68\\&69\\&69\end{align*}$$ | $$\begin{align*}&63\\&66\\&66\\&66\end{align*}$$ | $$\begin{align*}&61\\&61\\&61\\&61\end{align*}$$ |

#### Scenerio 4

<iframe src="https://drive.google.com/file/d/1feL0rFlWs3eQRyDO0LVLWCjqXS2i0fVL/preview" width="1000" height="800" allow="autoplay"></iframe>

| $$\begin{align*}&956\\&1799\\&1799\\&1799\end{align*}$$ | $$\begin{align*}&569\\&1113\\&1180\\&1799\end{align*}$$ | $$\begin{align*}&341\\&612\\&642\\&687\end{align*}$$ | $$\begin{align*}&256\\&435\\&455\\&486\end{align*}$$ | $$\begin{align*}&210\\&348\\&367\\&1799\end{align*}$$ |
| $$\begin{align*}&182\\&291\\&305\\&334\end{align*}$$ | $$\begin{align*}&162\\&252\\&264\\&285\end{align*}$$ | $$\begin{align*}&148\\&225\\&234\\&251\end{align*}$$ | $$\begin{align*}&137\\&204\\&212\\&225\end{align*}$$ | $$\begin{align*}&128\\&188\\&196\\&213\end{align*}$$ |
| $$\begin{align*}&121\\&175\\&181\\&195\end{align*}$$ | $$\begin{align*}&89\\&114\\&117\\&123\end{align*}$$ | $$\begin{align*}&79\\&94\\&96\\&100\end{align*}$$ | $$\begin{align*}&73\\&84\\&85\\&88\end{align*}$$ | $$\begin{align*}&70\\&78\\&79\\&81\end{align*}$$ |
| $$\begin{align*}&68\\&74\\&74\\&76\end{align*}$$ | $$\begin{align*}&66\\&71\\&71\\&72\end{align*}$$ | $$\begin{align*}&65\\&68\\&68\\&69\end{align*}$$ | $$\begin{align*}&63\\&66\\&66\\&67\end{align*}$$ | $$\begin{align*}&61\\&61\\&61\\&61\end{align*}$$ |

#### Remarks on Formula 4
