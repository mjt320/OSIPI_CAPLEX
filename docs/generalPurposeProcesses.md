
# <a name="Sections G"></a> Sections G: General purpose processes


## <a name="Forward model inversion"></a> Forward model inversion

### <a name="Inversion methods"></a> Inversion methods

| Code | OSIPI name| Alternative names|Notation|Description|Reference|
| -- | -- | -- | -- | -- | -- |
| G.MI1.001 | Analytical inversion | -- |  -- | This method is used when the solution of the model inversion is well-defined and the model parameters of interest can be calculated analytically. <br/> **Input:** <br /> [Forward model (M.GF1.001)](perfusionModels.md#Forward model), <br /> [Static model parameters (Q.AI1.001)](quantities.md#AI_SMP), <br /> [[Data (Q.GE1.002)](quantities.md#Data), [Data grid (Q.GE1.001)](quantities.md#DataGrid)] <br />**Output**: <br /> [[Model parameters (Q.OP1.001)](quantities.md#AI_MP)] | -- |
| G.MI1.002 | Optimization | Model fitting | -- |  Inversion of a forward model by iteratively adjusting the set of model parameters in order to minimize a similarity measure between the data and the model. <br/> **Input:** <br /> Optimizer (select from [optimizers](#Optimizers)) <br /> **Output**: <br /> [[Estimated model parameters (Q.OP1.003)](quantities.md#EMP)] | -- |
| G.MI1.003 | Deconvolution | -- | -- | Method which can be used when a model is given as a convolution $h(x) = f(x) \ast g(x)$ with known $h(x)$ and $f(x)$ to determine $g(x)$. <br/> **Input:** <br /> Deconvolution method (select from [deconvolution methods](#Deconvolution methods)) <br /> **Output**: <br />  [[Data (Q.GE1.002)](quantities.md#Data), [Data grid (Q.GE1.001](quantities.md#DataGrid))] = [g(x), x] | -- |
| G.MI1.999 | Method not listed | -- | -- |This is a custom free-text item, which can be used if a method of interest is not listed. Please state a literature reference and request the item to be added to the lexicon for future usage.  | -- |


## <a name="Optimization"></a> Optimization


### <a name="Optimizers"></a> Optimizers

| Code | OSIPI name| Alternative names|Notation|Description|Reference|
| -- | -- | -- | -- | -- | -- |
| G.OP1.001 | <a name="LM"></a> Levenberg-Marquardt | -- | LM | An algorithm that interpolates between the Gauss-Newton algorithm and the method of gradient descent.  <br/> **Input:** <br /> Cost function (select from [cost functions](#Cost functions)), <br /> [Initial model parameters (Q.OP1.006)](quantities.md#MP_init) <br /> *Optional:* <br />  [Model parameter lower bounds (Q.OP1.007)](quantities.md#MP_L), <br /> [Model parameter upper bounds (Q.OP1.008)](quantities.md#MP_U), <br /> [Data weights (Q.OP1.009)](quantities.md#Weights),<br /> [Maximum number of iterations (Q.OP1.010)](quantities.md#N_iter_max),<br /> [Convergence threshold (Q.OP1.011)](quantities.md#ConvThresh)<br /> **Output**: <br />  [Estimated model parameters (Q.OP1.003)](quantities.md#EMP),<br /> [Cost value minimum (Q.OP1.005)](quantities.md#CostValue_min)| -- |
| G.OP1.999 | <a name="not listed OP1"></a> Method not listed | -- | -- |This is a custom free-text item, which can be used if a method of interest is not listed. Please state a literature reference and request the item to be added to the lexicon for future usage.  | -- |


### <a name="Cost functions"></a> Cost functions

| Code | OSIPI name| Alternative names|Notation|Description|Reference|
| -- | -- | -- | -- | -- | -- |
| G.OP2.001 | <a name="NLLS"></a> Non-linear least squares  | -- | NLLS | $\lVert f(\phi, \theta; x) - y(x) \rVert_{2}^{2}$ , where *f* is a forward model describing the data, *x* is the data grid, *y(x)* is the measured data and $\lVert \rVert_2$ is the L2-norm. The forward model is non-linear in the model parameters. <br/> **Input:** <br /> [[Data (Q.GE1.002)](quantities.md#Data), [Data grid (Q.GE1.001)](quantities.md#DataGrid)], <br /> [Forward model (M.GF1.001)](perfusionModels.md#Forward model), <br /> [$\phi$ (Q.OP1.001)](quantities.md#MP), <br /> [$\theta$ (Q.OP1.002)](quantities.md#SMP) <br /> **Output**: <br /> [Cost value (Q.OP1.004)](quantities.md#CostValue) <br />  | -- |
| G.OP2.002 | <a name="LLS"></a> Linear least squares | -- | LLS | $\lVert A\phi - y(x) \rVert_2^2$ , where *x* is the data grid, *y(x)* is the measured data and $\lVert \rVert_2$ is the L2-norm. <br/> **Input:** <br /> [[Data (Q.GE1.002)](quantities.md#Data), [Data grid (Q.GE1.001)](quantities.md#DataGrid)], <br>[ $\phi$ (Q.OP1.001)](quantities.md#MP), <br /> [*A* (Q.OP1.012)](quantities.md#A) <br /> **Output**: <br /> [Cost value (Q.OP1.004)](quantities.md#CostValue) <br /> | -- |
| G.OP2.003 | <a name="SFT"></a> Standard-Form Tikhonov | -- | SFT | $\lVert A\phi - y(x) \rVert_2^2 + \lambda^2 \lVert Ix \rVert_2$ , where *x* is the data grid, *y(x)* is the measured data , $\lVert \rVert_2$ is the L2-norm and *L* is the identity matrix (same dimensions as *A*).  <br/> **Input:** <br /> [[Data (Q.GE1.002)](quantities.md#Data), [Data grid (Q.GE1.001)](quantities.md#DataGrid)], <br /> [$\phi$ (Q.OP1.001)](quantities.md#MP), <br /> [*A* (Q.OP1.012)](quantities.md#A), <br /> [$\lambda$ (Q.OP1.013)](quantities.md#Lambda) <br /> **Output**: <br /> [Cost value (Q.OP1.004)](quantities.md#CostValue) <br />| -- |
| G.OP2.004 | <a name="GCV"></a> Generalized cross validation | -- |GCV | $\frac{\lVert A\phi_\lambda - y(x) \rVert_2^2}{trace\left( I-AA_\lambda^\# \right)^2}$, <br>where *x* is the data grid, *y(x)* is the measured data, $\lVert \rVert_2$ is the L2-norm, *I* is the identity matrix of the same dimensions as *A*, $\phi_\lambda$ is the solution of the matrix equation obtained from the SVD for a certain regularization parameter $\lambda$ and $A^\#_\lambda$ is defined by the relationship <img src="https://latex.codecogs.com/svg.image?\phi_\lambda=A_{\lambda}^{\zeta}y(x)" title="https://latex.codecogs.com/svg.image?\phi_\lambda=A_{\lambda}^{\zeta}y(x)" />.  <br/> **Input:** <br /> [[Data (Q.GE1.002)](quantities.md#Data), [Data grid (Q.GE1.001)](quantities.md#DataGrid)], <br /> [<img src="https://latex.codecogs.com/svg.image?\phi&space;&space;" title="https://latex.codecogs.com/svg.image?\phi " /> (Q.OP1.001)](quantities.md#MP), <br /> [*A* (Q.OP1.012)](quantities.md#A), <br /> [<img src="https://latex.codecogs.com/svg.image?\lambda&space;" title="https://latex.codecogs.com/svg.image?\lambda " /> (Q.OP1.013)](quantities.md#Lambda) <br /> **Output**: <br /> [Cost value (Q.OP1.004)](quantities.md#CostValue) <br />| -- |
| G.OP2.005 | <a name="LC"></a> L-curve | -- |LC | TO DO <br/> **Input:** <br /> [[Data (Q.GE1.002)](quantities.md#Data), [Data grid (Q.GE1.001)](quantities.md#DataGrid)], <br /> [<img src="https://latex.codecogs.com/svg.image?\phi&space;&space;" title="https://latex.codecogs.com/svg.image?\phi " /> (Q.OP1.001)](quantities.md#MP), <br /> [*A* (Q.OP1.012)](quantities.md#A), <br /> [<img src="https://latex.codecogs.com/svg.image?\lambda&space;" title="https://latex.codecogs.com/svg.image?\lambda " /> (Q.OP1.013)](quantities.md#Lambda) <br /> **Output**: <br /> [Cost value (Q.OP1.004)](quantities.md#CostValue) <br />| -- |
| G.OP2.999 | <a name="not listed OP2"></a> Method not listed | -- | -- |This is a custom free-text item, which can be used if a method of interest is not listed. Please state a literature reference and request the item to be added to the lexicon for future usage.  | -- |


### <a name="Regularization parameter"></a> Regularization parameter

| Code | OSIPI name| Alternative names|Notation|Description|Reference|
| -- | -- | -- | -- | -- | -- |
| G.OP3.001 | <a name="Lambda_fixed"></a> Fixed | -- | -- | A fixed value of *&#955* , rather than a determined value is assumed. <br/> **Input:** <br /> [*&#955*<sub>fixed</sub> (Q.OP1.015)](quantities.md#Lambda_fixed) <br/> **Output:** <br /> [*&#955*(Q.OP1.013)](quantities.md#Lambda) | -- |
| G.OP3.002 | <a name="Lambda_GCV"></a> Generalized Cross Validation | -- | GCV | *&#955* is determined by minimizing the generalized cross validation cost function with respect to *&#955*. <br/> **Input:** <br /> Optimizer (select from [optimizers](#Optimizers)) with a [GCV cost function (G.OP2.004)](#GCV) and [*&#934*(Q.OP1.001)](quantities.md#MP) = [*&#955*(Q.OP1.013)](quantities.md#Lambda) <br/> **Output:** <br /> [*&#955*(Q.OP1.013)](quantities.md#Lambda)| -- |
| G.OP3.003 | <a name="Lambda_LC"></a> L-Curve criterion | -- | LCC | *&#955* is determined by minimizing the L-curve cost function with respect to *&#955*. <br/> **Input:** <br /> Optimizer  (select from [optimizers](#Optimizers)) with a  [L-curve cost function (G.OP2.005)](#LC) and [*&#934*(Q.OP1.001)](quantities.md#MP) = [*&#955*(Q.OP1.013)](quantities.md#Lambda). <br/> **Output:** <br /> [*&#955*(Q.OP1.013)](quantities.md#Lambda)| -- |
| G.OP3.999 | <a name="not listed OP3"></a> Method not listed | -- | -- |This is a custom free-text item, which can be used if a method of interest is not listed. Please state a literature reference and request the item to be added to the lexicon for future usage.  | -- |


## <a name="Deconvolution"></a> Deconvolution

| Code | OSIPI name| Alternative names|Notation|Description|Reference|
| -- | -- | -- | -- | -- | -- |
| G.DE1.001 | <a name="Discretization method"></a> Discretization method | -- | -- | Method to transfer continuous models, functions and equations into discrete counterparts. Select from [Discretization methods](#Discretization methods).| -- |
| G.DE1.002 | <a name="Regularization method"></a> Regularization method | -- | -- | Method to control an excessively fluctuating function such that the coefficients do not take extreme values. This is done by adding an additional penalty term in the cost function. Select a regularized cost function from [Cost functions](#Cost functions).| -- |
| G.DE1.999 | <a name="not listed DE1"></a> Method not listed | -- | -- |This is a custom free-text item, which can be used if a method of interest is not listed. Please state a literature reference and request the item to be added to the lexicon for future usage.  | -- |


### <a name="Deconvolution methods"></a> Deconvolution methods

| Code | OSIPI name| Alternative names|Notation|Description|Reference|
| -- | -- | -- | -- | -- | -- |
| G.DE2.001 |<a name="SVD"></a> Singular Value Decomposition | -- | SVD | Algebraic deconvolution of <img src="https://latex.codecogs.com/svg.image?h(x)=f(x)\ast&space;g(x)" title="https://latex.codecogs.com/svg.image?h(x)=f(x)\ast g(x)" /> with *f(x)* and *h(x)* sampled at discrete points *[f(x), x]* and *[g(x), x]*. The convolution equation is discretized according to a given discretization method and the resulting matrix equation is solved as a regularized least-squares problem with a given regularization method. <br/> **Input:** <br /> [[Data (Q.GE1.002)](quantities.md#Data), [Data grid (Q.GE1.001)](quantities.md#DataGrid)] =  [f(x), x], <br /> [[Data (Q.GE1.002)](quantities.md#Data), [Data grid (Q.GE1.001)](quantities.md#DataGrid)] = [g(x), x], <br /> Discretization method (select from [discretization methods](#Discretization methods) ), <br /> [Regularization method](#Regularization method) <br /> **Output**: <br />[[Data (Q.GE1.002)](quantities.md#Data), [Data grid (Q.GE1.001)](quantities.md#DataGrid)] = [g(x), x] <br />  | -- |
| G.DE2.999 | <a name="not listed DE2"></a> Method not listed | -- | -- |This is a custom free-text item, which can be used if a method of interest is not listed. Please state a literature reference and request the item to be added to the lexicon for future usage.  | -- |

### <a name="Discretization methods"></a> Discretization methods
In this group, the following notation is assumed for all functions *f*: *f<sub>n</sub>*= *f(x<sub>n</sub>)*.

| Code | OSIPI name| Alternative names|Notation|Description|Reference|
| -- | -- | -- | -- | -- | -- |
| G.DI1.001 | <a name="Numerical convolution 1st order"></a> Numerical convolution (first order)| -- | -- | Convolutions <img src="https://latex.codecogs.com/svg.image?f(x)=g(x)\ast&space;h(x)" title="https://latex.codecogs.com/svg.image?f(x)=g(x)\ast h(x)" /> are calculated as </br> <img src="https://latex.codecogs.com/svg.image?f_n&space;=&space;\Delta&space;x\cdot&space;\sum_{i=0}^{n}g_i\cdot&space;h_{n-i}" title="https://latex.codecogs.com/svg.image?f_n = \Delta x\cdot \sum_{i=0}^{n}g_i\cdot h_{n-i}" />  | -- |
| G.DI1.002 | <a name="Block-circulant"></a> Block-circulant | -- | -- | Convolutions <img src="https://latex.codecogs.com/svg.image?f(x)=g(x)\ast&space;h(x)" title="https://latex.codecogs.com/svg.image?f(x)=g(x)\ast h(x)" /> are calculated as <img src="https://latex.codecogs.com/svg.image?f_n&space;=&space;\Delta&space;x\cdot(g_0h_n&plus;g_1h_{n-1}&plus;...&plus;g_Nh_{n-N})" title="https://latex.codecogs.com/svg.image?f_n = \Delta x\cdot(g_0h_n+g_1h_{n-1}+...+g_Nh_{n-N})" /> . </br> This requires *h<sub>i</sub>* to be pre-padded with *N* zeros such that *h<sub>i</sub> < 0 = 0*.| Wu 2004 |
| G.DI1.003 | <a name="Volterra linear"></a> Volterra linear | -- | -- | Convolutions <img src="https://latex.codecogs.com/svg.image?f(x)=g(x)\ast&space;h(x)" title="https://latex.codecogs.com/svg.image?f(x)=g(x)\ast h(x)" /> are calculated as  </br> <img src="https://latex.codecogs.com/svg.image?f_n&space;=&space;\Delta&space;x\cdot&space;(h_0g_n^&plus;&space;&plus;\sum_{i=1}^{n-1}h_{n-i}g_i^{\pm}&plus;h_ng_0^&plus;)" title="https://latex.codecogs.com/svg.image?f_n = \Delta x\cdot (h_0g_n^+ +\sum_{i=1}^{n-1}h_{n-i}g_i^{\pm}+h_ng_0^+)" />| Sourbron 2003 |
| G.DI1.004 | <a name=Singular"></a> Singular | -- | -- | Convolutions <img src="https://latex.codecogs.com/svg.image?f(x)=g(x)\ast&space;h(x)" title="https://latex.codecogs.com/svg.image?f(x)=g(x)\ast h(x)" /> are calculated as </br> <img src="https://latex.codecogs.com/svg.image?f_n&space;=&space;\Delta&space;x\cdot(\sum_{i=-\infty&space;}^{&plus;\infty&space;}h_{n-i}g_i^\pm&space;)" title="https://latex.codecogs.com/svg.image?f_n = \Delta x\cdot(\sum_{i=-\infty }^{+\infty }h_{n-i}g_i^\pm )" /> . </br> It is assumed that *g<sub>i</sub> = 0* for *i < 0* and there is an index *k* such that h<sub>n</sub> = 0* for *n <- k*. | Ostergaard 1996 |
| G.DI1.005 | <a name=Hybrid"></a> Hybrid | -- | -- | Convolutions <img src="https://latex.codecogs.com/svg.image?f(x)=g(x)\ast&space;h(x)" title="https://latex.codecogs.com/svg.image?f(x)=g(x)\ast h(x)" /> are calculated as </br> <img src="https://latex.codecogs.com/svg.image?f_n&space;=&space;\frac{\Delta&space;x}{2}\cdot(\sum_{i=-\infty&space;}^{&plus;\infty}h_{n-i}(g_{i-1}&plus;g_i))" title="https://latex.codecogs.com/svg.image?f_n = \frac{\Delta x}{2}\cdot(\sum_{i=-\infty }^{+\infty}h_{n-i}(g_{i-1}+g_i))" /> .| Willats 2006 |
| G.DI1.006 | <a name=Exponential convolution"></a> Exponential convolution | --| --| Convolutions <img src="https://latex.codecogs.com/svg.image?f(x)&space;=&space;g(x)\ast&space;\frac{e^{-\frac{x}{T}}}{T}" title="https://latex.codecogs.com/svg.image?f(x) = g(x)\ast \frac{e^{-\frac{x}{T}}}{T}" /> are calculated as </br> <img src="https://latex.codecogs.com/svg.image?f_{i&plus;1}=e^{-x_i}f_i&plus;g_iE_0(x_i)&plus;a'TE_1(x_i)" title="https://latex.codecogs.com/svg.image?f_{i+1}=e^{-x_i}f_i+g_iE_0(x_i)+a'TE_1(x_i)" /> with </br> <img src="https://latex.codecogs.com/svg.image?x_i=\frac{t_{i&plus;1}-t_i}{T};&space;a'_i=\frac{a_{i&plus;1}-a}{t_{i&plus;1}-t_i};\&space;E_0&space;=&space;1-e^{-x};\&space;E_1(x)=x-E_0(x)" title="https://latex.codecogs.com/svg.image?x_i=\frac{t_{i+1}-t_i}{T}; a'_i=\frac{a_{i+1}-a}{t_{i+1}-t_i};\ E_0 = 1-e^{-x};\ E_1(x)=x-E_0(x)" /> . | Flouri 2016 |
| G.DI1.999 | <a name="not listed DI1"></a> Method not listed | -- | -- |This is a custom free-text item, which can be used if a method of interest is not listed. Please state a literature reference and request the item to be added to the lexicon for future usage.  | -- |



## <a name="Curve descriptive processes"></a> Curve descriptive processes
General processes applied to a given data set, e.g. processes to derive descriptive quantities are defined in this group.

| Code | OSIPI name| Alternative names|Notation|Description|Reference|
| -- | -- | -- | -- | -- | -- |
| G.CD1.001 | <a name="Calcf(x_i)"></a> Calculate value at data grid point | -- | *Calcf(x<sub>i</sub>)* | This process returns the data value *f(x<sub>i*</sub>) at the data grid point *x<sub>i</sub>*. <br /> **Input:** <br /> [[Data (Q.GE1.002)](quantities.md#Data), [Data grid (Q.GE1.001)](quantities.md#DataGrid)], <br /> [*i* (Q.GE1.003)](quantities.md#index) <br /> **Output**: <br /> [*f(x<sub>i</sub>)* (Q.CD1.001)](quantities.md#f(x_i)) | -- |
| G.CD1.002 | <a name="Calcf_max"></a> Calculate maximum of data | -- | *Calcf<sub>max</sub>*  | This process returns the maximum data value *f<sub>max</sub>* . <br /> **Input:** <br /> [Data (Q.GE1.002)](quantities.md#Data) <br /> **Output**: [<br /> *f<sub>max</sub>* (Q.CD1.002)](quantities.md#f_max) | --|
| G.CD1.003 | <a name="Calcx_max"></a> Calculate data grid point of maximum data value | -- | *Calcx<sub>max</sub>* | This process returns the data grid point at which the maximum of a given data set occurs. <br /> **Input:** <br /> [[Data (Q.GE1.002)](quantities.md#Data), [Data grid (Q.GE1.001)](quantities.md#DataGrid)] <br /> **Output**: <br /> [x<sub>max</sub> (Q.CD1.003)](quantities.md#x_max) | -- |
| G.CD1.004 | <a name="Calcf_min"></a> Calculate minimum of data | -- | *Calcf<sub>min</sub>*  | This process returns the minimum data value *f<sub>min</sub>* . <br /> **Input:** <br /> [Data (Q.GE1.002)](quantities.md#Data) <br /> **Output**: <br /> [*f<sub>min</sub>* (Q.CD1.004)](quantities.md#f_min) | --|
| G.CD1.005 | <a name="Calcx_min"></a> Calculate data grid point of minimum data value | -- | *Calcx<sub>min</sub>* | This process returns the data grid point at which the minimum of a given data set occurs. <br /> **Input:** <br /> [[Data (Q.GE1.002)](quantities.md#Data), [Data grid (Q.GE1.001)](quantities.md#DataGrid)] <br /> **Output**: <br /> [x<sub>min</sub> (Q.CD1.005)](quantities.md#x_min) | -- |
| G.CD1.006 | <a name="Calcf_fin"></a> Calculate value of final data point | -- | *Calcf<sub>fin</sub>* | This process returns the value of the data at the final data grid point. <br /> **Input:** <br /> [[Data (Q.GE1.002)](quantities.md#Data), [Data grid (Q.GE1.001)](quantities.md#DataGrid)] <br /> **Output**: <br /> [*f<sub>fin</sub>* (Q.CD1.006)](quantities.md#f_fin) | -- ||
| G.CD1.007 | <a name="Calcx_fin"></a> Calculate final data grid point | -- | *Calcx<sub>fin</sub>* | This process returns the last data grid point of a given data grid. <br /> **Input:** <br /> [Data grid (Q.GE1.001)](quantities.md#DataGrid) <br /> **Output**: <br /> [*x<sub>fin</sub>* (Q.CD1.007)](quantities.md#x_fin) | -- |
| G.CD1.008 | <a name="Calcf_BL_max"></a> Calculate maximum deviation from baseline | --| <img src="https://latex.codecogs.com/svg.image?Calc\Delta&space;f_{BL,max}" title="https://latex.codecogs.com/svg.image?Calc\Delta f_{BL,max}" />| This process returns the maximum absolute deviation of a given data set and baseline.  <br /> **Input:** <br /> [Data (Q.GE1.002)](quantities.md#Data), <br /> [Baseline value (Q.BL1.001)](quantities.md#f_BL) <br /> **Output**: <br /> [<img src="https://latex.codecogs.com/svg.image?\Delta&space;f_{BL,max}" title="https://latex.codecogs.com/svg.image?\Delta f_{BL,max}" /> (Q.CD1.008)](quantities.md#maxDev) | -- |
| G.CD1.009 | <a name="CalcDeriv"></a> Derivative at data grid point | -- | <img src="https://latex.codecogs.com/svg.image?Calc\frac{df(x_i)}{dx}" title="https://latex.codecogs.com/svg.image?Calc\frac{df(x_i)}{dx}" /> | This process returns the value of the derivative of a given data set at the data grid point x<sub>i</sub>. <br /> **Input:**<br /> [[Data (Q.GE1.002)](quantities.md#Data), [Data grid (Q.GE1.001)](quantities.md#DataGrid)], <br /> [i (Q.GE1.003)](quantities.md#index) <br />**Output**: <br /> [<img src="https://latex.codecogs.com/svg.image?\small&space;\frac{df(x_i)}{dx}" title="https://latex.codecogs.com/svg.image?\small \frac{df(x_i)}{dx}" /> (Q.CD1.009)](quantities.md#Deriv) <br />  | -- |
| G.CD1.010 | <a name="CalcTTP"></a> Calculate time to peak | -- | CalcTTP | This process returns the time to peak for a given bolus arrival time and data grid point of maximum value. <br /> **Input:** <br /> [*x<sub>max</sub>* (Q.CD1.003)](quantities.md#x_max),<br /> [*BAT* (Q.BA1.001)](quantities.md#BAT) <br /> **Output**:<br />  [*TTP* (Q.CD1.010)](quantities.md#TTP) | -- |
| G.CD1.011 | <a name="CalcWIS"></a> Calculate wash-in slope | -- | CalcWIS | This process returns the wash-in-slope for a given baseline, maximum value and time to peak of a data set. <br /> **Input:** <br /> [*f<sub>max</sub>* (Q.CD1.002)](quantities.md#f_max), <br /> [*f<sub>BL</sub>* (Q.BL1.001)](quantities.md#f_BL), <br /> [*TTP* (Q.CD1.010)](quantities.md#TTP)  <br /> **Output** : <br /> [*WIS* (Q.CD1.011)](quantities.md#WIS)  | -- |
| G.CD1.012 | <a name="CalcWOS"></a> Calculate wash-out slope | -- | CalcWOS | This process returns the wash-out-slope for a given maximum value, final data value and the data grid points of the maximum and final data value of a data set. <br /> **Input:** <br /> [*f<sub>max</sub>* (Q.CD1.002)](quantities.md#f_max), <br /> [*f<sub>fin</sub>* (Q.CD1.006)](quantities.md#f_fin), <br /> [*x<sub>max</sub>* (Q.CD1.003)](quantities.md#x_max), <br /> [*x<sub>fin</sub>* (Q.CD1.007)](quantities.md#x_fin) <br /> **Output**: <br/> [*WOS (Q.CD1.012)*](quantities.md#WOS)| -- |
| G.CD1.013 | <a name="CalcAUC"></a> Calculate area under curve  | -- | <img src="https://latex.codecogs.com/svg.image?CalcAUC_{x_{start},x_{end}}" title="https://latex.codecogs.com/svg.image?CalcAUC_{x_{start},x_{end}}" /> | This process returns the integral of data on a data grid in between a range of data grid points *x<sub>start</sub>* and *x<sub>end</sub>*. <br /> **Input:** <br /> [[Data (Q.GE1.002)](quantities.md#Data), [Data grid (Q.GE1.001)](quantities.md#DataGrid)],<br/> [[*x<sub>start</sub>* (Q.GE1.013)](quantities.md#x_start), [*x<sub>end</sub>*(Q.GE1.014)](quantities.md#x_end)] <br /> **Output**:<br /> [*AUC<sub>x<sub>start</sub>,x<sub>end</sub></sub>* (Q.CD1.013)](quantities.md#AUC) | -- |
| G.CD1.999 | <a name="not listed CD1"></a> Method not listed | -- | -- |This is a custom free-text item, which can be used if a method of interest is not listed. Please state a literature reference and request the item to be added to the lexicon for future usage.  | -- |



## <a name="Segmentation"></a> Segmentation
Processes related to segmentation are listed in this section.

| Code | OSIPI name| Alternative names|Notation|Description|Reference|
| -- | -- | -- | -- | -- | -- |
| G.SE1.001 | <a name=""></a> Create binary mask | -- | -- | This process creates a binary segmentation mask on a given data set using a specified segmentation method. <br /> **Input:** <br /> [Data (Q.GE1.002)](quantities.md#Data), <br /> Segmentation method (select from [segmentation methods](#Segmentation methods)) <br /> **Output**: <br /> [Binary mask (Q.SE1.001)](quantities.md#BinMask) | -- |
| G.SE1.002 | <a name=""></a> Apply binary mask | -- | -- | This process masks a given data set with a given mask. <br/> **Input:** <br /> [Data (Q.GE1.002)](quantities.md#Data), <br /> [Binary mask (Q.SE1.001)](quantities.md#BinMask) <br /> **Output**: <br /> [Data (Q.GE1.002)](quantities.md#Data) | --|
| G.SE1.999 | <a name="not listed SE1"></a> Method not listed | -- | -- |This is a custom free-text item, which can be used if a method of interest is not listed. Please state a literature reference and request the item to be added to the lexicon for future usage.  | -- |


### <a name="Segmentation methods"></a> Segmentation methods
| Code | OSIPI name| Alternative names|Notation|Description|Reference|
| -- | -- | -- | -- | -- | -- |
| G.SE2.001 | <a name="Freehand"></a> Freehand | -- | -- | Manual freehand drawing of contours. <br/> **Input:** <br />  [Data (Q.GE1.002)](quantities.md#Data) <br /> **Output**: <br /> [Binary mask (Q.SE1.001)](quantities.md#BinMask) | --|
| G.SE2.002 | <a name="Threshold"></a> Threshold | -- | -- | This method selects all input data with values in a specified range between lower and upper threshold. <br/> **Input:** <br /> [Data (Q.GE1.002)](quantities.md#Data), <br /> [Lower threshold (Q.GE1.010)](quantities.md#L), <br /> [Upper threshold (Q.GE1.011)](quantities.md#U) <br /> **Output**: <br/> [Binary mask (Q.SE1.001)](quantities.md#BinMask) | -- |
| G.SE2.003 | <a name=""></a> Region growing | -- | -- | This method grows a region from selected seeds with values between the lower and upper value threshold in the neighborhood of the seeds.  <br/> **Input:** <br /> [Data (Q.GE1.002)](quantities.md#Data), <br /> [Seeds (Q.SE1.004)](quantities.md#Seeds), <br /> [Lower threshold (Q.GE1.010)](quantities.md#L), <br /> [Upper threshold (Q.GE1.011)](quantities.md#U) <br /> **Output**: <br /> [Binary mask (Q.SE1.001)](quantities.md#BinMask) | -- |
| G.SE2.004 | <a name=""></a> *k*-means clustering | -- | -- | This method partitions the input data in a number of clusters using the *K*-means clustering algorithm and selects the cluster with the ith index as binary mask. <br/> **Input:** <br /> [Data (Q.GE1.002)](quantities.md#Data), <br /> [Number of *k*-Means clusters (Q.SE1.005)](quantities.md#N_kMeans), <br /> [*i* (Q.GE1.003)](quantities.md#index) <br /> **Output**: <br />  [Binary mask (Q.SE1.001)](quantities.md#BinMask) | -- |
| G.SE2.999 | <a name="not listed SE2"></a> Method not listed | -- | -- |This is a custom free-text item, which can be used if a method of interest is not listed. Please state a literature reference and request the item to be added to the lexicon for future usage.  | -- |


## <a name="Uncertainty estimation and statistics processes"></a> Uncertainty estimation
<b><font color=#FF0000>This section is currently work in progress</font></b> </br></br>

| Code | OSIPI name| Alternative names|Notation|Description|Reference|
| -- | -- | -- | -- | -- | -- |
| -- | -- | -- | -- | -- | -- |

## <a name="Averaging"></a> Averaging

| Code | OSIPI name| Alternative names|Notation|Description|Reference|
| -- | -- | -- | -- | -- | -- |
| G.AV1.001 | <a name=""></a> Calculate Average | -- | CalcAv | This process returns the average of input data according to a specified averaging method. <br/> **Input:** <br /> [Data (Q.GE1.002)](quantities.md#Data), Averaging method (select from [uncertainty estimation and statistics processes](#Uncertainty estimation and statistics processes) e.g. <img src="https://latex.codecogs.com/svg.image?\bar{x}" title="https://latex.codecogs.com/svg.image?\bar{x}" /> (G.US1.001) ) <br/> **Output:** <br /> [Data (Q.GE1.002)](quantities.md#Data)| -- |
| G.AV1.999 | <a name="not listed AV1"></a> Method not listed | -- | -- |This is a custom free-text item, which can be used if a method of interest is not listed. Please state a literature reference and request the item to be added to the lexicon for future usage.  | -- |












