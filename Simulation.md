# OUTPUT
![image](https://github.com/shreyasingh2302vl10/TCAD-SDE-Sentaurus-AlGaN-GaN-HEMT-Device-Design-/blob/94c3c718b4f59bc072f54ccda7f7dc3c5c9bfd2a/WhatsApp%20Image%202026-06-11%20at%2012.18.09%20AM.jpeg)
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
