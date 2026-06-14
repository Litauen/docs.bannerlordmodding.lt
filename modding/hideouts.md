# Hideouts

- [Hideout Scenes](https://docs.google.com/presentation/d/1bhTmkf9ti--o-Jc_qvikNLkVMQw0k5YO-UwtFX_eXaM/edit?usp=drive_link){target=_blank} by d&h


!!! quote "Cozur"

    Here's the guide to making custom hideouts work in War Sails:

    Step 1- Change levels to (in scene file):

    <levels>
        <level name="base" mask="1"/>
        <level name="level_1" mask="2"/>
        <level name="level_2" mask="4"/>
    </levels>


    Step 2 – Add Stealth Box (in editor). 10-15 entities seem to be a good fit?

    Step 3 – Add Dynamic Patrol Area (in scene file). 10-15 entities seem to be a good fit?

    Step 4 – change settlements.xml hideout composition to the naval version

    Step 5 – add the hideout thingie with arrows entity (stealth_area_use_point)