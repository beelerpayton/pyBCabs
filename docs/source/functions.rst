Functions for Internally Mixed Black Carbon Aggregates
=============================================================

Theory 
---------------------------------

   This package computes the core phase shift parameter (:math:`{\rho}`\ :sub:`BC`) to infer the morphology of fractal black carbon aggregates using the particle mass-equivalent diameter (d\ :sub:`p`), mass absorption cross-section (MAC\ :sub:`BC`), and mixing state (M\ :sub:`tot`/M\ :sub:`BC`). It should be noted that in this package, MAC\ :sub:`BC` is defined as the absorption cross-section per unit mass of black carbon. First, :math:`{\rho}`\ :sub:`BC` is constrained by determining whether the measured mass absorption cross-section (MAC\ :sub:`BC,meas`) is significantly less than that given by:
   
   :math:`{MAC_{BC,pred}=MAC_0\left (\frac{\lambda}{\lambda_0} \right)^{-AAE}\left[1+\frac{AC^{-B}\Gamma(B+1,C)}{C}-\frac{A\left(\frac{M_{tot}}{M_{BC}}\right)^{B}\left(\frac{M_{tot}}{M_{BC}}\right)^{-B}\Gamma\left(B+1,C\frac{M_{tot}}{M_{BC}}\right)}{C}\right]}`.	
	- :math:`{A=-1.189\pm0.029}`
	- :math:`{B=-0.674\pm0.006}`
	- :math:`{C=0.043\pm0.0007}`
	- :math:`{MAC_0=6.819\pm0.131}`
	- :math:`{AAE=1.231\pm0.005}`
	
   If MAC\ :sub:`BC,meas` is within 10% of MAC\ :sub:`BC,pred`, then :math:`{\rho}`\ :sub:`BC` can be constrained to 0 < :math:`{\rho}`\ :sub:`BC` < 1, but cannot be exactly calculated. If MAC\ :sub:`BC,meas` is less than 90% of MAC\ :sub:`BC,pred`, then :math:`{\rho}`\ :sub:`BC` is calculated by solving:   
   
   :math:`{MAC_{BC,meas}=MAC_0\left (\frac{\lambda}{\lambda_0} \right)^{-AAE}\left[\frac{D}{E+1}\left(\rho_{BC}^{1-E}-1\right)+\frac{D}{1-2E}\left(\rho_{BC}^{1-2E}-1\right)\right]+MAC_{BC,pred}}`.
   
   Where :math:`{D}` and :math:`{E}` are sigmoid functions, given by:
   
   	:math:`{X=x_1+\frac{x_2-x_1}{1+\text{exp}\left[x_3\left(\rho_{BC}-x_4\right)\right]}}`.
	
   Here :math:`{X}` represents :math:`{D}` or :math:`{E}`, and :math:`{x_{[1,2,3,4]}}` represents :math:`{d_{[1,2,3,4]}}` or :math:`{e_{[1,2,3,4]}}`. 
   
	- :math:`{d_1=5.679\pm0.027}`
	- :math:`{d_2=1.066\pm0.058}`
	- :math:`{d_3=0.264\pm0.010}`
	- :math:`{d_4=11.421\pm0.137}`
	- :math:`{e_1=2.440\pm0.017}`
	- :math:`{e_2=0.593\pm0.024}`
	- :math:`{e_3=0.418\pm0.020}`
	- :math:`{e_4=10.106\pm0.131}`
   
   
   Details on the derivation of the above equations are `available here <https://doi.org/10.1016/j.jqsrt.2017.10.012>`_.
   
   The morphology of of the measured black carbon aggregates can be determined by comparing the calculated :math:`{\rho}`\ :sub:`BC` to three cases. The first case is that of freshly emitted black carbon, which has fractal dimension (D\ :sub:`f`) of 1.8. The second case is black carbon which has partially collapsed, and has D\ :sub:`f` of 2.5. The final case is black carbon which has fully collapsed (but not sintered), and has D\ :sub:`f` of 3.0. 
   
   The core phase shift parameter of black carbon aggregates with morphologies outlined above is found by first determining their radius of gyration R\ :sub:`g`, given by:
   
	:math:`{R_g=a\left(\frac{m_p}{m_1 k_f}\right)^{\frac{1}{D_f}}}`,
	
   where :math:`{a}` is the monomer radius, k\ :sub:`f` is the fractal prefactor (fixed at 1.2), m\ :sub:`p` is the black carbon mass, and m\ :sub:`1` is the mass of a BC monomer. Both the BC mass and the monomer mass are determined assuming BC density of 1.8 g/cm\ :sup:`3`. Next, the monomer packing fraction :math:`{\left(\phi\right)}` is found using:
   
	:math:`{\phi=k_f\left(\frac{D_f+2}{D_f}\right)^{-\frac{3}{2}}\left(\frac{a}{R_g}\right)^{3-D_f}}`.
	
   Finally, :math:`{\rho}`\ :sub:`BC` is given by:

	:math:`{\rho_{BC}=\frac{4\pi R_g}{\lambda}\left|m_{eff}-1\right|}`,
	
   where :math:`{m_{eff}}` is given by:
   
	:math:`{\phi\left(\frac{m^2-1}{m^2+2}\right)=\left(\frac{m_{eff}^2-1}{m_{eff}^2+2}\right)}`.

   Here, :math:`{m}` is the refractive index of black carbon, :math:`{1.95+0.79i}`. The core phase shift parameter and mass of the measured black carbon aggregates is then compared to the three cases described above, allowing for inference of particle morphology.
   
   In a similar manner, users can supply the morphology, mixing state, and particle diameter, and the mass absorption cross-section can be calculated based on the core phase shift parameter.


Inverse function for single particle
---------------------------------

.. py:function:: abs2shape_SP(diameter, coating, absorption, wavelength, [k_coat=0.0, abs_error=0.0, mode='MtotMbc', r_monomer=20, asDict=True, ReturnPlot=True, PlotPoint=True])

   Black carbon mass-equivalent diameter, coating amount, and MAC\ :sub:`BC` are input and morpholgy is inferred using the procedure outlined `above <https://pyBCabs.readthedocs.io/en/latest/functions.html#theory>`_. The particle mass is used to infer the number of monomers, assuming the density of black carbon is 1.8 g/cm\ :sup:`3`.
   
   **Parameters**
   
   diameter : float
	Black carbon mass-equivalent diameter with units of nm.
   coating: 
	Coating amount with units matching that of the optional 'mode' input. Default is ratio of total particle mass to black carbon mass.
   absorption:
	MAC\ :sub:`BC` with units of m\ :sup:`2`/g.
   wavelength : float
	The wavelength of incident light, in nanometers.
   k_coat : float
	Imaginary part of coating refractive index.
   abs_error : float, optional
	Error associated with measurement of MAC\ :sub:`BC`, in  m\ :sup:`2`/g.
   mode : string, optional
	- 'Mtot_Mbc' : ratio of total particle mass to black carbon mass.
	- 'Rbc' : ratio of coating mass to black carbon mass.
	- 'OC:EC' : ratio of organic carbon mass to black carbon mass.
	- 'percent_BC' : percentage of total particle mass which is attributed to black carbon.
   r_monomer : float, optional
	Radius of monomers, in nanometers.
   asDict : bool, optional
	If true, returns dict of output variables.
   ReturnPlot : bool, optional
	If true, returns figure and axes with morphology retrival plot.
   PlotPoint : bool, optional
	If true, shows measured particle on morphology retrival plot.
	
   **Returns**
   
   fig, ax : figure, axes
	Figure and axes with morphology retrival plot. If PlotPoint==True, then particle is shown on morphology retrieval plot.
   mass : float
	Mass of particle, in fg.
   rho_lower : float
	Lower limit of core phase shift parameter, based on average MAC\ :sub:`BC` and MAC\ :sub:`BC` errors.
   rho_avg : float
	Average core phase shift parameter, based on average MAC\ :sub:`BC`.
   rho_upper : float
	Upper limit of core phase shift parameter, based on average MAC\ :sub:`BC` and MAC\ :sub:`BC` errors.
	
Forward function for single particle
---------------------------------

.. py:function:: shape2abs_SP(dp, coating, collapse, wavelength, [k_coat=0.0, mode='MtotMbc', r_monomer=20, asDict=True])

   Black carbon mass-equivalent diameter, coating amount, and morphology are input and MAC\ :sub:`BC` is calculated using the procedure outlined `above <https://pyBCabs.readthedocs.io/en/latest/functions.html#theory>`_.
   
   **Parameters**
   
   dp : float
	Black carbon mass-equivalent diameter with units of nm.
   coating : float
	Coating amount with units matching that of the optional 'mode' input. Default is ratio of total particle mass to black carbon mass.
   collapse : string
   	- 'fresh' : black carbon morphology matches fresh soot with fractal dimension of 1.8.
	- 'partial' : black carbon core has partially collapsed, fractal dimension of 2.5.
	- 'full' : black carbon core has fully collapsed, fractal dimension of 3.0.
   wavelength : float
	The wavelength of incident light, in nanometers.
   k_coat : float
	- Imaginary part of coating refractive index. 
   mode : string, optional
	- 'Mtot_Mbc' : ratio of total particle mass to black carbon mass
	- 'Rbc' : ratio of coating mass to black carbon mass
	- 'OC:EC' : ratio of organic carbon mass to black carbon mass
	- 'percent_BC' : percentage of total particle mass which is attributed to black carbon.
   r_monomer : float, optional
	Radius of monomers, in nanometers.
   asDict : bool, optional
	If true, returns dict of output variables.
	
   **Returns**
   
   dp : float
	Mass-equivalent diameter of particle in nm.
   coating : float
	Amount of coating with same units as input.
   MAC : float
	MAC\ :sub:`BC` with units of m\ :sup:`2`/g.

Inverse function for black carbon size distribution
---------------------------------

.. py:function:: abs2shape_SD(dpg, sigma_g, coating, absorption, wavelength, [k_coat=0.0, abs_error=0.0, mode='MtotMbc', r_monomer=20, asDict=True, ReturnPlot=True])

   Black carbon mass-equivalent lognormal size distribution, coating amount, and MAC\ :sub:`BC` are input and morpholgy is inferred using the procedure outlined `above <https://pyBCabs.readthedocs.io/en/latest/functions.html#theory>`_. The particle mass is used to infer the number of monomers, assuming the density of black carbon is 1.8 g/cm\ :sup:`3`.
   
   **Parameters**

   dpg : float
	Black carbon geometric mean mass-equivalent diameter of lognormal distribution with units of nm.
   sigma_g : float
	Geometric standard deviation of black carbon lognormal size distribution.
   coating: 
	Coating amount with units matching that of the optional 'mode' input. Default is ratio of total particle mass to black carbon mass.
   absorption:
	MAC\ :sub:`BC` with units of m\ :sup:`2`/g.
   wavelength : float
	The wavelength of incident light, in nanometers.
   k_coat : float
	- Imaginary part of coating refractive index.   
   abs_error : float, optional
	- Error associated with measurement of MAC\ :sub:`BC`, in  m\ :sup:`2`/g.
   mode : string, optional
	- 'Mtot_Mbc' : ratio of total particle mass to black carbon mass.
	- 'Rbc' : ratio of coating mass to black carbon mass.
	- 'OC:EC' : ratio of organic carbon mass to black carbon mass.
	- 'percent_BC' : percentage of total particle mass which is attributed to black carbon.
   r_monomer : float, optional
	Radius of monomers, in nanometers.
   asDict : bool, optional
	If true, returns dict of output variables.
   ReturnPlot : bool, optional
	If true, returns figure and axes with morphology retrival plot.
	
   **Returns**
   
   fig, ax : figure, axes
	If ReturnPlot==True, figure and axes with morphology retrival plot.
   lower_mass : float
	Average-standard deviation of mass of particles, in fg.
   avg_mass : float
	Average mass of particles, in fg.
   upper_mass : float
	Average+standard deviation of mass of particles, in fg.
   rho_lower : float
	Lower limit of core phase shift parameter, based on average MAC\ :sub:`BC` and MAC\ :sub:`BC` errors.
   rho_avg : float
	Average core phase shift parameter, based on average MAC\ :sub:`BC`.
   rho_upper : float
	Upper limit of core phase shift parameter, based on average MAC\ :sub:`BC` and MAC\ :sub:`BC` errors.

Forward function for black carbon size distribution
---------------------------------

.. py:function:: shape2abs_SD(dpg, sigma_g, coating_avg, coating_stdev, collapse, wavelength, [k_coat=0.00, mode='MtotMbc', r_monomer=20, DataPoints=False, ShowPlots=True])

   Black carbon mass-equivalent lognormal size distribution, coating distribution, and morphology are input and distribution of MAC\ :sub:`BC` is calculated. Black carbon mass-equivalent diameter and coating amount are randomly sampled per the input distributions, and MAC\ :sub:`BC` is calculated using the procedure outlined `above <https://pyBCabs.readthedocs.io/en/latest/functions.html#theory>`_. 
   
   **Parameters**
   
   dpg : float
	Black carbon geometric mean mass-equivalent diameter of lognormal distribution with units of nm.
   sigma_g : float
	Geometric standard deviation of black carbon lognormal size distribution
   coating_avg : float
	Average value of coating amount, assuming a Gaussian normal distribution. Units should match that of the optional 'mode' input, default is ratio of total particle mass to black carbon mass.
   coating_stdev : float
   	Stabdard deviation of coating amount, assuming a Gaussian normal distribution.
   collapse : string
   	- 'fresh' : black carbon morphology matches fresh soot with fractal dimension of 1.8.
	- 'partial' : black carbon core has partially collapsed, fractal dimension of 2.5.
	- 'full' : black carbon core has fully collapsed, fractal dimension of 3.0.
   wavelength : float
	The wavelength of incident light, in nanometers.
   k_coat : float
	- Imaginary part of coating refractive index.
   mode : string, optional
	- 'Mtot_Mbc' : ratio of total particle mass to black carbon mass
	- 'Rbc' : ratio of coating mass to black carbon mass
	- 'OC:EC' : ratio of organic carbon mass to black carbon mass
	- 'percent_BC' : percentage of total particle mass which is attributed to black carbon.
   r_monomer : float, optional
	Radius of monomers, in nanometers.
   DataPoints : bool, optional
	If true, returns dict of output variables.
   ShowPlots : bool, optional
	If true, shows histograms of input parameters and calculated MAC\ :sub:`BC`.
	
   **Returns**
   
   dp : float
   	- If DataPoints==True, mass-equivalent black carbon diameters used in calculations, in nm.
	- If DataPoints==False, average and standard deviation of mass-equivalent black carbon diameters used in calculations, in nm.
   coating : float
   	- If DataPoints==True, coating amounts used in calculations, with units matching those of 'mode' option.
	- If DataPoints==False, average and standard deviation of coating amounts used in calculations, with units matching those of 'mode' option.
   MAC : float
   	- If DataPoints==True, calculated MAC\ :sub:`BC` values, in  m\ :sup:`2`/g.
	- If DataPoints==False, average and standard deviation of calculated MAC\ :sub:`BC` values, in  m\ :sup:`2`/g.
