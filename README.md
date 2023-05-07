Download Link: https://assignmentchef.com/product/solved-embsys110-assignment-4-traffic-light
<br>



After gaining experience in implementing a given statechart with QP in the previous assignment, students will have a chance to <em>design</em> and <em>implement</em> a statechart from given requirements.

<h1>2         Readings</h1>

<ol start="2013">

 <li><strong>Systems of Systems Modeled by a Hierarchical Part-Whole State-Based Formalism</strong>. Luca Pazzi. June 2013.</li>

</ol>

<a href="https://www.researchgate.net/publication/236156084_Systems_of_Systems_Modeled_by_a_Hierarchical_Part-Whole_State-Based_Formalism">(</a><a href="https://www.researchgate.net/publication/236156084_Systems_of_Systems_Modeled_by_a_Hierarchical_Part-Whole_State-Based_Formalism">https://www.researchgate.net/publication/236156084_Systems_of_Systems_Modeled_by_a_Hi </a><a href="https://www.researchgate.net/publication/236156084_Systems_of_Systems_Modeled_by_a_Hierarchical_Part-Whole_State-Based_Formalism">erarchical_Part-Whole_State-Based_Formalism</a><a href="https://www.researchgate.net/publication/236156084_Systems_of_Systems_Modeled_by_a_Hierarchical_Part-Whole_State-Based_Formalism">)</a>

This paper discusses how to partition a system into layers of HSMs. At each layer, an HSM is a <strong><em>part</em></strong> working for the HSM at the layer above it, while at the same time acts as a <strong><em>whole</em></strong> representing its constituent HSMs at the layer below it.

Fig. 3 shows the traffic light system similar to that we use in this assignment. The controller (bottom part – Note it’s upside down) is a part to the bigger system while it is a whole representing the two lights it controls/synchronizes.

Fig. 2 shows the counter-example without using this hierarchical control pattern in which two traffic lights interacting directly to each other without an explicit controller.

<ol start="2008">

 <li><strong>From Protocol Specification to Statechart to Implementation</strong>. Kiri L. Wagstaff, Ken Peters, and Lucas Scharenbroich. Jet Propulsion Laboratory. October 2008.</li>

</ol>

<a href="http://citeseerx.ist.psu.edu/viewdoc/download;jsessionid=81E437D3F3CEAC9F0D1B7FD187AC709D?doi=10.1.1.207.5154&amp;rep=rep1&amp;type=pdf">(</a><a href="http://citeseerx.ist.psu.edu/viewdoc/download;jsessionid=81E437D3F3CEAC9F0D1B7FD187AC709D?doi=10.1.1.207.5154&amp;rep=rep1&amp;type=pdf">http://citeseerx.ist.psu.edu/viewdoc/download;jsessionid=81E437D3F3CEAC9F0D1B7FD187 </a><a href="http://citeseerx.ist.psu.edu/viewdoc/download;jsessionid=81E437D3F3CEAC9F0D1B7FD187AC709D?doi=10.1.1.207.5154&amp;rep=rep1&amp;type=pdf">AC709D?doi=10.1.1.207.5154&amp;rep=rep1&amp;type=pdf</a><a href="http://citeseerx.ist.psu.edu/viewdoc/download;jsessionid=81E437D3F3CEAC9F0D1B7FD187AC709D?doi=10.1.1.207.5154&amp;rep=rep1&amp;type=pdf">)</a>

Like (a) above this paper uses a traffic light system to illustrate statecharts and orthogonal regions. The Traffic component in our assignment project is similar to Figure 2 of this paper.

<h1>3         Setup</h1>

<ol>

 <li><em>Carefully</em> unplug any extension modules from your Nucleo board. They are not required for this project.</li>

 <li>Download the compressed project file (platform-stm32f401-nucleo_assignment4.tgz) from the Assignment 4 course site.</li>

</ol>

Place the downloaded tgz file in your VM under <strong>~/Projects/stm32</strong>.

<ol start="3">

 <li>Move your existing project folder to a backup folder, e.g.</li>

</ol>

mv platform-stm32f401-nucleo platform-stm32f401-nucleo.bak

Note – You may want to pick another backup folder name if the one shown above already exists.

<ol start="4">

 <li>Decompress the tgz file with:</li>

</ol>

tar xvzf platform-stm32f401-nucleo_assignment4.tgz

The project folder will be expanded to <strong>~/Projects/stm32/platform-stm32f401-nucleo</strong>.

<ol start="5">

 <li>Launch Eclipse. Hit F5 to refresh the project content.</li>

</ol>

Or you can right-click on the project in Project Explorer and then click “Refresh”.

<ol start="6">

 <li>Clean and rebuild the project. Download it to the board and make sure it runs.</li>

</ol>

In minicom, type the command “traffic n” or “traffic e”. You should see log messages printed out on the console.

<ol start="7">

 <li>If you have not installed UMLet, you will need to do so for this assignment. Please refer to the Setup notes in the previous assignment.</li>

</ol>

<h1>4         Tasks</h1>

The design used in this assignment is for a traffic light system at an intersection between North-South (NS) and East-West (EW) streets.

There are two sets of traffic lights, one along the NS direction and the other along the EW direction. They are illustrated in the diagram below. Note that there are two opposite facing traffic lights in each set, which show the same pattern (both RED or both GREEN, etc). For brevity, only one in each set (south facing and east facing) is shown below. Also for simplicity there are no left-turn signals.

South




<ol>

 <li>In the assignment project, the traffic light system is located in the folder <strong>src/Traffic</strong>. It contains the Traffic active object and the Lamp orthogonal regions (under src/Traffic/Lamp”).</li>

</ol>

The event interface for the Traffic component is defined in TrafficInterface.h. The event TRAFFIC_CAR_NS_REQ is a request from an external component (e.g. smart camera sensor) to request passage along the NS direction. The event is generated each time when a car is detected in front of the north or south facing traffic light. The similar concept applies to the EW direction. For simplicity, we can assume that the camera sensor is smart enough to generate only one event for each specific car it detects.

For simulation, the console command ‘<strong>traffic</strong> <strong>n</strong>’ or ‘<strong>traffic</strong> <strong>s</strong>’ generates a TRAFFIC_CAR_NS_REQ event, and ‘<strong>traffic</strong> <strong>e</strong>’ or ‘<strong>traffic</strong> <strong>w</strong>’ generates a TRAFFIC_CAR_EW_REQ event.

<ol start="2">

 <li>The reference design is described in the statechart named Traffic.uxf. Your are tasked to improve it according to the following new requirements:

  <ul>

   <li>Imposes a minimum duration between each change of direction to avoid the “go” direction from toggling too fast. Note that the NS direction is hypothetically busier and its minimum “go” duration of 20s is intentionally longer than that of the EW direction of 10s.</li>

   <li>Since the NS direction is the main route, we would like to have the “go” direction automatically return to the NS direction when <em>no cars have been detected along the EW direction for 15 seconds</em>. Note: You can use the event TRAFFIC_CAR_EW_REQ to check if a car has passed through the intersection along the EW direction.</li>

   <li>Do you see another problem with the original design? What if a TRAFFIC_CAR_EW_REQ event arrives in the EWSlow state? That is a EW-going car is detected while the yellow light is shown along the EW direction. (Assume the driver behaves well and stops in front of the light rather than rushing through it.) Try to fix this issue to avoid the car from being stuck in front of the light forever.</li>

  </ul></li>

 <li>Show your design changes in the Traffic statechart. Update the Traffic.uxf file with UMLet and export the finished design to a pdf file for submission.</li>

 <li>Test your code with the console command “traffic …” and ensure the expected log messages are observed.</li>

</ol>