# Hex>Grid Manager Tutorial #

This tutorial will guide you through creating a simple demonstration of Hex>Grid Manager's features. By the end of this tutorial, you should have an understanding of how to utilize this plugin in your own projects.

## Table of Contents ##

[Hex>Grid Manager Tutorial](#hexgrid-manager-tutorial)

* [New Project](#new-project)
* [Basic Setup](#basic-setup)
* [Mouse/Touch Events](#use-mousetouch-events)
* [Overlap Events](#use-overlap-events)
* [Other Examples](#other-examples)
* [Help](#help)

&nbsp;

## New Project ##

Create a new, **blank project**:\
![New Project](./images/tutorial_01_new_project.png "ToolTip_Message")

&nbsp;

### Import Plugin ###

Import the Hex>Grid Manager into the new project.

&nbsp;

## Basic Setup ##

Create a new, **basic level** (File > New Level):\
![New Level](./images/tutorial_02_new_level.png "ToolTip_Message")

&nbsp;

### Place GridManager in Level ###

From the **Content Drawer**, navigate to "*All > Plugins > GridManagerBasic Content > Blueprints*", then drag **BP_HexGridManager**:\
![Add GridManager](./images/tutorial_03_add_gridmanager.png "ToolTip_Message")

&nbsp;

and drop it into the new level:\
![Continue Adding GridManager](./images/tutorial_04_add_gridmanager2.png "ToolTip_Message")

&nbsp;

Configure the Hex>GridManager instance by selecting it and setting the *Details > Hex Grid* options:\
![Configure GridManager](./images/tutorial_05_configure_gridmanager.png "ToolTip_Message")

&nbsp;

(Optionally, click the **Spawn Tiles** button to spawn editor-only tiles in the editor.)\
![Spawn in Editor](./images/tutorial_06_spawn_in_editor.png "ToolTip_Message")

&nbsp;

### Spawn Tiles ###

To spawn the tiles in-game, we need to call the [Spawn Tiles](../../documentation/documentation.md#spawntiles) Blueprint function. **Open the Level Blueprint** by opening the world blueprints drop-down menu, and selecting '*Open Level Blueprint*' (from the Level editor):\
![Open Level Blueprint](./images/tutorial_07_open_level_blueprint.png "ToolTip_Message")

&nbsp;

Right-click on the *Event Graph*, and select, "*Call Function on BP Hex Grid Manager > Hex Grid > Spawn Tiles*":\
![Place SpawnTiles Node](./images/tutorial_08_place_spawntiles_node.png "ToolTip_Message")

&nbsp;

Then drag the execute pin from the *Event BeginPlay* node to the [Spawn Tiles](../../documentation/documentation.md#spawntiles) node.\
![Connect SpawnTiles Node](./images/tutorial_09_connect_spawntiles_node.png "ToolTip_Message")

&nbsp;

Now, when you compile and play this level, a tile grid will spawn. Save the level:\
![Save Level](./images/tutorial_10_save_level.png "ToolTip_Message")

&nbsp;

## Use Mouse/Touch Events ##

**It is necessary to [Enable Mouse/Touch Events](../enable_mousetouch_events/enable_mousetouch_events.md) before any related Events will fire.**

There are four, independently-firing Mouse events, and the same is true for Touch events. With a little logic, events from both can be grouped into a common event lifecycle:

| Begin | Enter | Leave | End |
|---|---|---|---|
|Clicked|BeginCursorOver|EndCursorOver|Released|
|TouchBegin|TouchEnter|TouchLeave|TouchEnd|

&nbsp;

Let's build a demo that uses all of these events.

### Mouse Events ###

From the *Level Blueprint Event Graph*, with the **BP_HexGridManager** selected on the Level editor, create the *OnTileClicked* Event node by right-clicking the Event Graph and selecting "*Add Event for BP Hex Grid Manager > Event Dispatchers > Add On Tile Clicked*":\
![Image_Title](./images/tutorial_20_add_onclicked_node.png "ToolTip_Message")

Drag the "Touched Tile" pin out to create a new node, search for "set material", and select "Set Material (TileMesh)":\
![Image_Title](./images/tutorial_21_set_onclicked_node1.png "ToolTip_Message")

Drag the execute pin from the "*On Tile Clicked*" Event node to the "*Set Material*" node, and set Material to *BasicAsset01*:\
![Image_Title](./images/tutorial_22_set_onclicked_node2.png "ToolTip_Message")

Repeat the same steps with *On Tile Begin Cursor Over*, (use *BasicShapeMaterial*):\
![Image_Title](./images/tutorial_23_add_onbegincursorover_node.png "ToolTip_Message")

And with *On Tile End Cursor Over* (use ArrowMaterial):\
![Image_Title](./images/tutorial_24_add_onendcursorover_node.png "ToolTip_Message")

And *On Tile Released* (BasicAsset03):\
![Image_Title](./images/tutorial_25_add_onrelease_node.png "ToolTip_Message")

If you Compile & Run the Level now, you should be able to see the events in action. You may find the default camera controls to be unhelpful for this testing. You can [set up a static camera](../static_camera/static_camera.md) to deal with this. More complicated examples are available in the Level Blueprint for the example Level included with this plugin. The specifics of when these events fire is explained in the [documentation](../../documentation/documentation.md#events).

&nbsp;

### Touch Events ###

To group mouse and touch events, it is necessary to convert our event responses into Macros. Select our response to *On Tile Clicked*, right-click one of the nodes, and select "*Collapse to Macro*":\
![Image_Title](./images/tutorial_26_collapse_to_macro.png "ToolTip_Message")

Name this Macro, "Begin":\
![Image_Title](./images/tutorial_27_name_macro.png "ToolTip_Message")

Set up the other three Macros:\
![Image_Title](./images/tutorial_28_setup_macros.png "ToolTip_Message")

Now, Add all four **BP_HexGridManager** touch events to the Level Blueprint, and connect them to the macros you just created. Additional macro nodes can be placed on the Event Graph by dragging them from the Macros section of the *My Blueprint* side-bar.\
![Image_Title](./images/tutorial_29_add_touch_events.png "ToolTip_Message")

&nbsp;

#### Use Mouse To Simulate Touch ####

To test this on you PC, you can set the Project to [use the mouse for touch](../use_mouse_for_touch/use_mouse_for_touch.md).

&nbsp;

## Use Overlap Events ##

There are two overlap events available for use; Begin Overlap & End Overlap. In order for them to function, the project and actors involved will need to have a Collision Box and have overlap events enabled.

### Add Collision Box ###

Adding a collision box to an actor is simple. Edit the actor blueprint (Content Drawer, double-click, or right-click, edit) and open the "*Viewport*". On the *Components* tool-bar, select the RootComponent and then click the "Add" button. Type "Box" and then select, "*Box Collision*":\
![Image_Title](./images/tutorial_33_add_boxcollision.png "ToolTip_Message")

Use the *Viewport* to adjust the *Box Collision* appropriately for the expected use. You may want to set 'Visible' to false on the Box Collision if you are spawning the actor in the level editor. Be sure to compile & save:\
![Image_Title](./images/tutorial_34_adjust_boxcollision.png "ToolTip_Message")

### Add Sphere Actor ###

To test overlap events, lets add another actor to the level. From the Content Drawer, in *All > Content > Blueprints*, create a new Blueprint (BP_Sphere) with Actor as the Parent Class, and then edit the new Blueprint. From the *Viewport*, add a *Sphere* and a *Box Collision* (be sure to compile & save):\
![Image_Title](./images/tutorial_35_create_sphere_actor.png "ToolTip_Message")

We're going to implement a very simple mouse-move mechanic in order to test overlap events. On **BP_Sphere**'s Event Graph, add a new **Boolean** variable named *IsBeingMovedByMouse*, and then add the following function:\
![Image_Title](./images/tutorial_36_setmoving_function.png "ToolTip_Message")

Subscribe to relevant mouse and touch events to keep the *IsBeingMovedByMouse* variable updated:\
![Image_Title](./images/tutorial_37_sphere_actor_mouse.png "ToolTip_Message")

Add the following nodes to the Event Graph, and then you should be able to drag the sphere around:\
![Image_Title](./images/tutorial_38_simple_move_to_mouse.png "ToolTip_Message")

### Overlap Events ###

Let's subscribe to the overlap events and add some reactions:\
![Image_Title](./images/tutorial_39_add_overlap_events.png "ToolTip_Message")

Disable the Mouse/Touch event responses and play the level. When you drag the Sphere Actor through any tile, it should produce a reaction. You may need to adjust the static distance Float to produce an overlap with a tile. If the events aren't firing, check that "*Generate Overlap Events*" is set to true under *Details > Collision* for the Mesh and Box Collision of the Tile and Actor.

## Other Examples ##

The following Blueprints demonstrate features of the Hex>Grid Manager plugin. All Events and Functions are [documented here](../../documentation/documentation.md).

&nbsp;

Function: Set Material for (an Array of) Tiles.\
![Image_Title](./images/tutorial_40_settilesmaterial.png "ToolTip_Message")

Set Material for all Tiles.\
![Image_Title](./images/tutorial_41_setalltilesmaterial.png "ToolTip_Message")

Set Material for Neighbor Tiles.\
![Image_Title](./images/tutorial_43_setneighborsmaterial.png "ToolTip_Message")

Set Material for Tile at a specific distance.\
![Image_Title](./images/tutorial_44_setneighborsmaterialatdistance.png "ToolTip_Message")

Move Actor to Tile.\
![Image_Title](./images/tutorial_42_moveactortotile.png "ToolTip_Message")

On the Level Blueprint Event Graph, combine all of the functions into a response to Tile Clicked.
![Image_Title](./images/tutorial_45_combined_response.png "ToolTip_Message")\
![Image_Title](./images/tutorial_46_combined_response_play.png "ToolTip_Message")

&nbsp;

## Help ##

* **Documentation:** Available [here](./documents/documentation/documentation.md).
* **Tutorials:** Learn the basics with these [tutorials](./documents/tutorials/README.md).
* **Videos:** Coming soon on [YouTube](https://www.youtube.com/@Mj12Studio).
* **Discord:** Join our community on [Discord](https://discord.gg/2SsKNeHY3u).
