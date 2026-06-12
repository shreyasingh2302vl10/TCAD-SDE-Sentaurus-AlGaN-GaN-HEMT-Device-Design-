# OUTPUT- Id vs Vg
![image](https://github.com/shreyasingh2302vl10/TCAD-SDE-Sentaurus-AlGaN-GaN-HEMT-Device-Design-/blob/00b3b4dc8aabc114010edbf2db62279afcb0a62c/id_vg_correct.png)
# CODE -SDevice TOOL id_vg
```scheme

File {
  Grid      = "@tdr@"
  Parameter = "@parameter@"
  Current   = "@plot@"
  Plot      = "@tdrdat@"
  Output    = "@log@"
}


Electrode{
{ Name="s" Voltage=0.0}
{ Name="d" Voltage=0.7}
{ Name="g" Voltage=0.0}
}

Physics(Material="Silicon"){
Fermi
EffectiveIntrinsicDensity(OldSlotboom)
Mobility(
PhuMob
Enormal
eHighFieldsat(CarrierTempDrive)
hHighFieldsat(GradQuasiFermi)
* thin_layer_mobility
)

Recombination(
SRH(DopingDependence)
eAvalanche(CarrierTempDrive)
hAvalanche(CarrierTempDrive)
)

}

Plot{
eDensity hDensity eCurrent hCurrent
Potential SpaceCharge ElectricField
eMobility hMobility eVelocity hVelocity
Doping DonorConcentration AcceptorConcentration
}

Math{
Extrapolate
RelErrControl
-CheckUndefinedModels
}

Solve{
Poisson
Coupled { Poisson Electron }
Quasistationary ( MaxStep=0.05
Goal{ Name="g" Voltage=10 } )
{ Coupled { Poisson Electron } }
}
```
