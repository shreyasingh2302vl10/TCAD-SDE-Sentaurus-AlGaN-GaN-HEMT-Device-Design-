# OPTIONS
![image](https://github.com/shreyasingh2302vl10/TCAD-SDE-Sentaurus-AlGaN-GaN-HEMT-Device-Design-/blob/9a32b328c7a1c26cc21ee3d8119b43ec20a7be80/WhatsApp%20Image%202026-06-11%20at%201.20.13%20AM%20(1).jpeg)
# CODE 
```scheme
* ==============================================================================
* FULLY MAPPED SDEVICE CODE FOR P-GATE HEMT (MATCHED WITH SDE)
* ==============================================================================

File {
  Grid      = "@tdr@"
  Parameter = "@parameter@"
  Current   = "@plot@"
  Plot      = "@tdrdat@"
  Output    = "@log@"
}

* --- 1. ELECTRODE DEFINITIONS ---
* S and D are Ohmic (Voltage=0). G is Schottky on top of p-GaN (Aluminum workfunction ~ 4.1 to 5.1 eV)
Electrode {
  { Name="S" Voltage= 0.0 }
  { Name="D" Voltage= 0.0 }
  { Name="G" Voltage= 0.0 Workfunction= 5.1 Schottky } 
}

* --- 2. GLOBAL PHYSICS ---
Physics {
  Temperature= 300.0
  Fermi
  
  * THE FIX: Yeh line TCAD ko deep acceptors calculate karne ko bolegi
  *IncompleteIonization
  
  Mobility(
    DopingDependence(Masetti)
    HighFieldSaturation(GradQuasiFermi)
  )
  
  Recombination(
    SRH
    Radiative
  )
}

* --- 3. REGION SPECIFIC PHYSICS (THE 2DEG & TRAPS) ---
* Creating the 2DEG exactly at the interface of your 'barrier' (AlGaN) and 'buffer' (GaN)
Physics (RegionInterface="barrier/buffer") {
  Piezoelectric_Polarization(strain Activation= 1.0)
  
  * Interface traps (Essential for GaN convergence and realistic currents)
  Traps (
    Donor Level EnergyMid= 0.4 fromCondBand Conc= 3e13
    eXSection= 1e-14 hXSection= 1e-14
  )
}

* --- 4. ADVANCED MATH & SOLVER (THE CRASH PREVENTER) ---
Math {
  ExtendedPrecision(256)
  Digits= 6
  Iterations= 20
  Notdamped= 50
  Transient= BE
  
  * Prevents wild oscillations from deep traps
  Traps(Damping=0) 
  
  ErrRef(electron) = 1e8
  ErrRef(hole)     = 1e8
  CDensityMin      = 0
  
  * Robust Blocked matrix solver for Wide-Bandgap materials
  Method= Blocked
  SubMethod= ILS(set=27)
  ILSrc="
  set(27){  
    iterative(gmres(100), tolrel=1e-10, tolunprec=1e-4, tolabs=0, maxit=200);
    preconditioning(ilut(1.0e-9,-1), right);
    ordering(symmetric=nd, nonsymmetric=mpsilst);
    options(compact=yes, linscale=0, refineresidual=10, verbose=0);
  };
  "
}

* --- 5. SOLVE SECTION (TURNING THE DEVICE ON) ---
Solve {
  * Step 1: Initial Zero-Bias Solution (Equilibrium)
  Coupled(Iterations=100) { Poisson }
  Coupled { Poisson Electron Hole }
  
  * Step 2: Gate Voltage Sweep (Quasistationary)
  * Sweeps the Gate (G) from 0V to 2.0V
  Quasistationary (
    InitialStep= 0.05 MaxStep= 0.1 MinStep= 1e-5
    Goal { Name="G" Voltage= 2.0 }
  ) { Coupled { Poisson Electron Hole } }
  
  * Step 3: Drain Voltage Sweep (Transient for Stability)
  * Sweeps the Drain (D) from 0V to 10.0V to get the Id-Vd curve
  Transient (
    InitialStep= 0.01 MinStep= 1.0e-5 maxstep= 0.1
    goal { name="D" voltage= 10.0 }
  ) { 
    Coupled { Poisson Electron Hole } 
  }
}
