## File Schema
Each line in the file is one instance of damage. It is a comma-delimited (`,`) string with the following fields:  
1. **Player Category** (`Caster`/`Healer`/`Melee`/`Ranged`/`Tank`)
1. **Timestamp** of the damage event, relative to fight start time (milliseconds)
1. **Ability id**
1. **Ability name**
1. Raw **damage taken** from the ability (the number that you see in game)
1. The total sum of all the **shield values** that have broken from the damage
1. **Damage type**, `P`/`M`/`D` for physical/magical/darkness damage respectively
1. **Position** of the player that took the damage, in the form `x|y`
1. **Position** of the actor that caused the damage, in the form `x|y`
1. A **list of mitigation** from player buffs (see below)
1. A **list of mitigation** from debuffs on the enemy (see below)
1. **Debug info** in the following form: `DEBUG:<reportId>_<fightNumber>:<playerName>(<playerId>):<timestamp>`
   * You can visit the log from where the damage originated by going to the following link:  
     `https://www.fflogs.com/reports/<reportId>#fight=<fightNumber>`
   * The `timestamp` is an absolute timestamp for all the fights in the report

## Mitigation Lists
A mitigation list is a semicolon-delimited (`;`) string of *mitigations*.  
A *mitigation* is a pipe-separated (`|`) string with the following fields:
1. Mitigation name
1. Mitigation id
1. Damage multiplier for magical damage (`M`)
1. Damage multiplier for physical damage (`P`)
1. Intervention bonus (`I`)

The full list of defined mitigations is defined below. For a mitigation like Reprisal which reduces damage by 10%, the physical and magical multipliers are both `0.9`. For a mitigation like Feint that only reduces physical damage by 10%, the magical multiplier is `1` and the physical multiplier is `0.9`.

The following formula will calculate the total mitigation for physical damage (replace `P` with `M` for magical damage):  

                Total_Mitigation = (P_1-I_1) * (P_2-I_2) * ... * (P_n-I_n)

The purpose of storing these mitigations individually is to make it possible to filter damage events based on what mitigation is applied, in order to verify if some particular mitigation works on an ability or not.

### Mitigation Definitions
These are the base mitigations from buffs/debuffs:
```
Sentinel|1000074|0.7|0.7|0 
Shield Wall|1000194|0.8|0.8|0 
Last Bastion|1000196|0.2|0.2|0 
Sacred Soil|1000299|0.9|0.9|0 
Fey Illumination|1000317|0.95|1|0 
Raw Intuition|1000735|0.8|0.8|0 
Dark Mind|1000746|0.8|1|0 
Shadow Wall|1000747|0.7|0.7|0 
Collective Unconscious|1000849|0.9|0.9|0 
Land Waker|1000863|0.2|0.2|0 
Dark Force|1000864|0.2|0.2|0 
Vulnerability Down (Vengeance)|1000912|0.7|0.7|0 
Intervention|1001174|0.9|0.9|0 
Arms Up|1001176|0.85|0.85|0 
Riddle of Earth|1001179|0.9|0.9|0 
Rampart|1001191|0.8|0.8|0 
Reprisal|1001193|0.9|0.9|0 
Feint|1001195|1|0.9|0 
Addle|1001203|0.9|1|0 
Wheel of Fortune|1001206|0.9|0.9|0 
Third Eye|1001232|0.9|0.9|0
Shield Samba|1001826|0.9|0.9|0 
Camouflage|1001832|0.9|0.9|0 
Nebula|1001834|0.7|0.7|0 
Heart of Light|1001839|0.9|1|0 
Heart of Stone|1001840|0.85|0.85|0 
Nascent Glint|1001858|0.9|0.9|0 
Temperance|1001873|0.9|0.9|0 
Seraphic Illumination|1001875|0.95|1|0 
Dark Missionary|1001894|0.9|1|0 
Gunmetal Soul|1001931|0.2|0.2|0 
Troubadour|1001934|0.9|0.9|0 
Tactician|1001951|0.9|0.9|0 
```

**Note:** Block and Parry are also considered mitigations, and they are defined as follows:
```
Blocked|4|0.8|0.8|0 
Parry|20|1|0.85|0 
```
