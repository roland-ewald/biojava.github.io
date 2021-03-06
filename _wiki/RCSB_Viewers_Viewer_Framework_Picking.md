---
title: RCSB Viewers:Viewer Framework:Picking
permalink: wiki/RCSB_Viewers%3AViewer_Framework%3APicking
---

Notes
-----

-   `     This is an interesting mechanism, but is fiddly and subject to rendering errors if the back buffer happens to get swapped and still has the picking colors - you get the odd 'red-shift' effect.  A normal redraw fixes it.`  
    `   `

</li>
</ul>
Questions
---------

-   `     Where does the dummy context get set up? - I'm conjecturing this happens, because it's the only possible`  
    `     solution in my comprehension, but I haven't tracked it down.`  
    `   `
-   `     What is the action that is forwarded on successful pick?`  
    `   `
-   `     Should this be replaced with an actual ray-pick?  I doubt if it would be any more expensive than`  
    `     the 'glReadPixel' calls (which are quite expensive), and would avoid the afore-mentioned 'red-shift'`  
    `     effect.`

Relevent Classes
----------------

-   GlGeometryViewer

Explanation
-----------

Picking is achieved by intercepting mouse movements and then initiating
a redraw, after setting a flag, indicating that the requested draw is
actually a pick request.

The technique is based on a 'unique-color' mechanism, rather than a
'ray-pick' mechanism (See *OpenGL Programming Guide, Sixth Ed. - Object
Selection Using the Back Buffer*)

On the redraw event, the action is forwarded to several layers of
'PickOrRedraw' functions. If picking, the execution path sets up a
'unique color' scheme - essentially, the material for each pickable
object type is set to a unique color (starting with 1, 0, 0 - dark red)
and that association is set in a lookup table by color (color -\>
StructureComponent.)

After rendering to the back buffer, the pixel at the mouse location is
read (with a <em>glReadPixel</em>) and the color looked up in the table.
If it is found and is associated with a <em>StructureComponent</em>
object, that object is set as the currently picked object.
