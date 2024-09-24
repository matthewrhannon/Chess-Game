# Chess-Game
The AI along with various quirks are still under development. This program is a combination between Chess3D (OpenGL 3.3) and ChessBase (Console C++).

*** Update on Chess3D ***

I have combined Chessbase and Chess3D to make a beta version of Chess
which is an interactive chess game. The piece movement is still very buggy, and the
AI is only beginning to be implemented.

The updated controls to move pieces around on the board are the following:

   To initiate a move:
    o Press ‘r’ to reset the camera view
    o Press ‘m’ to initiate move sequence
    o Right click piece you would like to move (It will highlight)
    o Then right click the square/piece you would like to move to.
    o If the play was valid, the highlighted piece should have moved. If
      the move did not occur then view the console or log.txt output to
      understand why, and then repeat these steps.

Idea behind project:
The idea behind Chess3D came from my ultimate goal which is to develop an
interactive chess game. The major goals that I set out with at the beginning of this
project were to have an accurate rendering of each of the pieces, and each piece had to
be selectable via the mouse. Furthermore the pieces had to be smart enough to always
be inside one of the cells in an 8x8 chess grid. This would require a conversion
function for world coordinates to grid coordinates. Another important aspect was to
develop Chess3D in a way that it would be relatively easy to combine with the
ChessBase code. This combination of Chess3D and ChessBase has not yet been
implemented to help simplify the delivery of this project because it would require
porting ChessBase to Linux and this project is primarily about graphics design.

Meeting project specifications:
The scene in Chess3D is a relatively interesting 3D scene with a full Phong
lighting environment which is able to be dynamically rotated, panned, and zoomed.
One procedural texture is employed which draws a circle on the piece that is right
clicked by the mouse. There are two image-based textures from external files which
are ‘jade.bmp’ for the board and ‘blackmarble.bmp’ for the chess pieces. Depth and
double buffering are used. My initial projection mode is perspective but this can be
changed by pressing the ‘p’ key which alternates between perspective and
orthographic projection. The ESC key can be used to exit the program. Material
properties are specified for all the pieces and the board. The material properties can be
found in the ‘doInitialize’ routine in both Piece3D.cpp and Board3D.cpp. The Phong
lighting model implementation was adopted from the ShapeDemo vertex and
fragment shader. There are two light sources in my scene. One light source is a
spotlight which points towards the world coordinate (0, 0, -1) , and the other light
source is a directional source with a position of (0, 1, 0). Both these lighting variable
assignments can be found in the Lighting.cpp file.

The piece models for Chess3D were found by searching the internet for free
good looking chess models. A chess set matching these qualities was found at the site
in reference 1. A problem was quickly noticed when I found that whoever created this
set created it as a whole object rather than individual pieces which can be moved
independently of others. This issue was bypassed by using the free modeling program
called MilkShape 3D. Using this program I separated each type of piece from each
other as well as the board. I also used MilkShape to generate texture coordinates and
normal vectors for each vertex defining the model.

Interactive Controls:
I implemented keyboard and mouse controls which allow the user to select a
piece on the board and visually see the piece that was selected. This control can be
accessed by clicking the right mouse button over a piece. If the mouse is not over a
piece but over a square without a piece, the grid X and Y coordinate can still be
viewed in the console output. The ability to accurately select a grid cell based on the
mouse cursor required setting a specific camera setup which the conversion from
mouse coordinates to grid coordinates assumes is the current projection setup.
Therefore another control was added to reset the camera to this default setup so the
mouse to grid conversion is accurate, and the user can still rotate, pan, and zoom the
scene. This control can be accessed by pressing the ‘r’ key which stands for reset.
Pressing the ‘r’ key will assume a perspective projection and will reset all the camera
values to their original settings. In this scene setup, the right mouse click on a
particular square should be accurately transformed into the proper grid coordinate.

Difficult things while developing:
I had a lot of issues when trying to implement the ability to transform a mouse
cursor position into a grid position. I attempted to implement a version of
gluUnproject based on available source code on the internet. I had problems adapting
this source code to a version that would work with my program. Once gluUnproject
was working as a function inside of the Camera class, I next turned to how I was
going to change the world coordinate values into accurate positions on the grid. It
took me awhile to figure out that I would need to agree on some kind of known
camera configuration from which to base the linear conversion that I developed by
reading the boundary values for the 8x8 grid which were then used to calculate the
board world coordinate dimensions. These dimensions were divided by 8, and this
information combined with the size of an individual cell was used to map the values
into the range of 1 to 8. The current implementation is still only valid after the ‘r’ key
is pressed if any camera alterations have occurred. Another problem that still occurs is
the camera orientation sometimes jumps suddenly when clicking the right mouse
button.

Unique items in project:
I used the OBJ file format for the models. The code I used to read the OBJ file
was based on the code given in the forums of the website called GameDev. A link to
this site can be found in reference two as well as the OBJ_Loader.cpp file. Another
maybe not so unique but helpful item in Chess3D is that it has separate classes for the
lighting model, the texture loading system, and the camera. This really helped with
project organization.

There are currently three supported values for a modelview variable called
‘iColor’ which is used to tell the shader of a particular piece whether it is a black,
white, or a selected piece. This color alternation is accomplished inside of the
fragment shader for all the pieces. The ‘iColor’ board value is initialized to -1
currently but will later be used to select visually individual grid cells that are not
occupied by a chess piece.

References
1) chessboard2. TurboSquid, n.d. Web. 3 Dec. 2011.
<http://www.turbosquid.com/3d-models/chess-chessboard-lwo-free/251828>.

3) OBJ Model Loading (In Detail). N.p., n.d. Web. 4 Dec. 2011.
<http://www.gamedev.net/topic/312335-obj-model-loading-in-detail/>.
