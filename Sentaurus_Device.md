# Structure From the Research Paper
![image](https://github.com/shreyasingh2302vl10/TCAD-SDE-Sentaurus-AlGaN-GaN-HEMT-Device-Design-/blob/80f8984e9c0d2c9090b5ca1401e72bd1e723f8d5/WhatsApp%20Image%202026-06-03%20at%203.56.20%20PM%20(1).jpeg)
# CODE -SDE tool
```scheme
;===================================================================================
;                1. PARAMETERS (DIMENSIONS & THICKNESS)
;===================================================================================

(define LS    3)   ; Source Length
(define LGS   1.5) ; Gate-to-Source Spacer
(define LG    1.5) ; Main Gate Length
(define LGD   7)   ; Gate-to-Drain Spacer
(define LD    3)   ; Drain Length

(define Tsub  2)   ; Scaled Silicon Substrate Thickness for Mesh Stability
(define Tbuf  2)   ; GaN Buffer Thickness
(define Tbar  0.02); 20 nm AlGaN Barrier Thickness
(define TpGaN 0.1) ; 100 nm p-GaN Layer Thickness
(define Tgate 0.05); Main Gate Metal Thickness

; Total Source/Drain Thickness Calculation (Elevated)
(define Textra 0.3)   
(define Tsource (+ (+ TpGaN Tgate) Textra))

;===================================================================================
;                2. COORDINATES (X-AXIS & +Y DOWN-TO-UP AXIS)
;===================================================================================

; Horizontal X-Axis Points
(define x0 0)
(define x1 (+ x0 LS))
(define x2 (+ x1 LGS))
(define x3 (+ x2 LG))
(define x4 (+ x3 LGD))
(define x5 (+ x4 LD))

; Converted Vertical +Y Axis Points
(define y0 0)                ; Absolute Device Bottom
(define y1 (+ y0 Tsub))       ; Top of Si Substrate / Base of GaN Buffer
(define y2 (+ y1 Tbuf))       ; Top of GaN Buffer / Base of AlGaN Barrier
(define y3 (+ y2 Tbar))       ; Top of AlGaN Barrier (Base for S, p-GaN, D)
(define y4 (+ y3 TpGaN))      ; Top of p-GaN layer (Bottom of Main Gate Metal)
(define y5 (+ y4 Tgate))      ; Top of Main Gate Metal
(define y6 (+ y3 Tsource))    ; Top surface of Source/Drain (Absolute Ceiling)

;===================================================================================
;                3. STRUCTURE & GEOMETRY GENERATION
;===================================================================================

; LEVEL 1: Si Substrate
(sdegeo:create-rectangle (position x0 y0 0) (position x5 y1 0) "Silicon" "substrate")

; LEVEL 2: GaN Buffer Layer
(sdegeo:create-rectangle (position x0 y1 0) (position x5 y2 0) "GaN" "buffer")

; LEVEL 3: AlGaN Barrier Layer
(sdegeo:create-rectangle (position x0 y2 0) (position x5 y3 0) "AlGaN" "barrier")

; LEVEL 4, 5 & EXTRA (Left): Source Contact Block
(sdegeo:create-rectangle (position x0 y3 0) (position x1 y6 0) "GaN" "source_region")

; LEVEL 4 (Center): P-GaN Layer
(sdegeo:create-rectangle (position x2 y3 0) (position x3 y4 0) "GaN" "p_gate")

; LEVEL 5 (Center): Main Gate Metal Electrode
(sdegeo:create-rectangle (position x2 y4 0) (position x3 y5 0) "Aluminum" "gate_metal")

; LEVEL 4, 5 & EXTRA (Right): Drain Contact Block
(sdegeo:create-rectangle (position x4 y3 0) (position x5 y6 0) "GaN" "drain_region")

;===================================================================================
;            4. DOPING PROFILES (FIXED FOR INCOMPLETE IONIZATION)
;===================================================================================

; --- LEVEL 1: Silicon Substrate Doping ---
(sdedr:define-constant-profile "Substrate_Background_Profile" "BoronActiveConcentration" 1e15)
(sdedr:define-constant-profile-region "Substrate_Doping_Assign" "Substrate_Background_Profile" "substrate")

; --- LEVEL 2: GaN Buffer Layer Doping ---
(sdedr:define-constant-profile "GaN_Background_Profile" "PhosphorusActiveConcentration" 1e15)
(sdedr:define-constant-profile-region "Buffer_Doping_Assign" "GaN_Background_Profile" "buffer")

; --- LEVEL 4: P-GaN Gate Layer Doping (THE MAGIC FIX) ---
; Yeh TCAD ko batayega ki yeh Magnesium hai, Boron nahi!
(sdedr:define-constant-profile "Mg_pGaN_Profile" "MagnesiumActiveConcentration" 3e17)
(sdedr:define-constant-profile-region "pGaN_Doping_Assign" "Mg_pGaN_Profile" "p_gate")

; --- LEVEL 4/5 & EXTRA: Source & Drain Heavily Doped Ohmic Regions ---
(sdedr:define-constant-profile "Heavy_n_Profile" "PhosphorusActiveConcentration" 1e19)
(sdedr:define-constant-profile-region "Source_Doping_Assign" "Heavy_n_Profile" "source_region")
(sdedr:define-constant-profile-region "Drain_Doping_Assign" "Heavy_n_Profile" "drain_region")

;===================================================================================
;                5. GUI NATIVE BOUNDARY CONTACT SETS
;===================================================================================

; Define Contact Sets with colors (Red for Drain, Yellow for Source, Magenta for Gate)
(sdegeo:define-contact-set "D" 4 (color:rgb 1 0 0) "##")
(sdegeo:define-contact-set "S" 4 (color:rgb 1 1 0) "##")
(sdegeo:define-contact-set "G" 4 (color:rgb 1 0 1) "##")

; --- SOURCE CONTACT ASSIGNMENT ---
(sdegeo:set-current-contact-set "S")
; Finds edge exactly at the middle of Source top surface (x0 to x1, height y6)
(sdegeo:set-contact (list (car (find-edge-id (position (/ (+ x0 x1) 2) y6 0)))) "S")

; --- DRAIN CONTACT ASSIGNMENT ---
(sdegeo:set-current-contact-set "D")
; Finds edge exactly at the middle of Drain top surface (x4 to x5, height y6)
(sdegeo:set-contact (list (car (find-edge-id (position (/ (+ x4 x5) 2) y6 0)))) "D")

; --- GATE CONTACT ASSIGNMENT ---
(sdegeo:set-current-contact-set "G")
; Finds edge exactly at the middle of Gate top metal surface (x2 to x3, height y5)
(sdegeo:set-contact (list (car (find-edge-id (position (/ (+ x2 x3) 2) y5 0)))) "G")

;===================================================================================
;                6. MESH REFINEMENT & GUI SAVE 
;===================================================================================

(sdedr:define-refinement-size "RefDef" 0.2 0.2 0.01 0.01)

(sdedr:define-refinement-placement "RefGaN" "RefDef" (list "region" "buffer"))
(sdedr:define-refinement-placement "RefBarrier" "RefDef" (list "region" "barrier"))
(sdedr:define-refinement-placement "RefGate" "RefDef" (list "region" "p_gate"))

; Save boundary model for SWB GUI representation
(sde:save-model "n@node@")
(sde:build-mesh "n@node@")
```
# STRUCTURE MADE ON SENTAURUS TOOL 
![image](https://github.com/shreyasingh2302vl10/TCAD-SDE-Sentaurus-AlGaN-GaN-HEMT-Device-Design-/blob/a9dc02d0bad3015e0c6c33e7eff88a5b1cd753f1/Structure_Device.jpeg)
