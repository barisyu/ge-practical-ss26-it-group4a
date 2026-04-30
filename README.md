# Lab Report - V050

## Project Information

- **Practical Course:** General Enginnering Practicum 
- **Lab name:** Informatik v50
- **Date:** 27.04.2026
- **Group Members:**  
  - Baris Yaksi
  - Sofia Atti  
  - Qiye Jiang
  - Ze Xen Choong
  - Zohreh Farhadi
- Instructors: Prof. Rasche & J. Schweizer B.Sc.

---

________________________________________
Carla And Yolov8 Integration Lab Report Group 4a Ss26
📌 Introduction
One main aim here: connect a fake self-driving setup with instant image analysis. Inside CARLA, the system grabs video from a driver-piloted car. That feed moves straight into a YOLOv8 model for spotting objects. Instead of waiting, everything runs as the scene unfolds. Vision data does not sit idle - it flows. The toolchain links simulation eyes to detection logic. No delays built in by design. Processing kicks off the moment pixels arrive. From synthetic road to neural guess - happens in step.
What drove us forward wasn’t just spotting objects in still images, but catching them on the move - vehicles, people - in real time, while keeping the simulation smooth and responsive. This step showed how self-driving tech begins to make sense of surroundings, layer by layer, opening doors to smarter reactions such as sudden stops or dodging crashes.
Setup and Tools
A setup on Windows formed the base for our workspace, where an Anaconda virtual environment handled tricky package needs. While coding began, dependency conflicts were kept under control through isolated environments. The machine stayed dedicated to tasks without interference from system-level changes. Each tool found its place within the structure created by Conda's isolation features.
Running on version 0.9.15 or 0.9.16 of the CARLA Simulator. CarlaUE4.exe handles execution behind the scenes. The simulation environment uses this setup to operate smoothly. Engine core powered entirely by these specific builds.
Environment Anaconda yolo sim with Python.

•	Core Libraries:
Inside carla (.whl) lies a toolset that connects directly to the simulated environment. From there, characters appear when called into the scene. Sensors come alive through its control paths. Interaction flows via structured access points rather than loose hooks.
pygame managed simulation visuals controls and interface elements.
Ultralytics supplied YOLOv8 for detecting objects.
Image handling leans on cv2 for shifting between RGB and BWR formats when needed.
From numpy, work began on shaping data straight from sensors - turning bytes into usable arrays. Transformations unfolded through structured number handling, each step built around clean layout control.
Working with pathlib kept file paths flexible, adapting smoothly across different systems when pulling in models.
Out front, the yolov8n.pt (Nano) model - already trained - kept performance smooth when paired with CARLA's demanding 3D environment. Running fast mattered, so we leaned on that ready-made setup instead of building one from scratch. Frame speed stayed strong thanks to this lightweight choice working beside the simulation.

📝 Exercise Summary
We tackled it step by step, breaking things apart first. Then came testing pieces together slowly over time. Each stage built on what happened just before. Moving forward only after checking results carefully. The process unfolded in layers, one at a time
1. Starting at localhost:2000, the initial link was confirmed. After that came loading Town01 into view. A Tesla Model 3 appeared next, placed by command. Instead of default settings, the surroundings shifted - weather changed to HardRainNight. All steps ran through the Jupyter Notebook interface.
2. Picture this - just one frame at a time. The game visuals now split off cleanly from detection code. Instead of tangled functions, each part stands alone. A snapshot from Pygame gets reshaped into something OpenCV can handle without fuss. That grid of numbers? Sent straight to YOLO like mail through a slot. Once there, boxes form around objects, sharp and clear. Works the same whether it is still or moving. Detection rides on top, quiet and precise.
3. A different perspective took shape in Exercise 4 - the last step. Our reasoning shifted directly into the primary manual_control.py file. Rather than depend on several floating windows, a tailored setup emerged: the "Sandwich Rendering Pipeline." That change redefined how visuals came together
From up high, the view now shows us plus surrounding cars, since we changed to third-person mode (_transform_index = 0). Seeing everything unfolds like that helps track motion beyond just our own vehicle.
We took the raw Pygame display data before adding the HUD.
Flipping the axes along with the color channels opened the step before feeding into YOLO. After inference finished, transformation brought the labeled frame back as a Pygame surface. Only then did the process complete.
Back on the display went the AI-labeled picture, followed by an overlay of CARLA’s built-in readouts - speed plus frame rate. Critical info stayed visible, never buried under detection outlines.
Challenges and What We Learned
Midway through linking everything up, a few glitches showed up - each one got fixed right away
Even though we were in the right terminal, the error kept appearing - ModuleNotFoundError: No module named 'carla'. Turns out, on Windows PowerShell, the system tends to stick with the (base) Python version, ignoring the conda environment shown up top. What worked wasn’t fixing the shell but going around it entirely. Instead of relying on the usual command line flow, we pointed straight at the specific interpreter hidden inside our setup: C:\Users\BarisY\anaconda3\envs\yolo-sim\python.exe manual_control.py.
Script stopped working when moving between computers because the file path was fixed. Instead of a rigid address, we used Python’s pathlib to find where the script lives on any machine. This change lets the program discover its own home folder automatically. Now the YOLO model loads correctly no matter which lab computer runs it. The fix follows a clue from the guide but applies it directly in practice.
Images get handled in separate ways by Pygame and OpenCV - width and height might flip, colors shift between red-blue orders. Feeding unadjusted screen output straight into YOLO twisted what the model saw. To fix it, we flipped array dimensions using swapaxes on axes zero and one. Then converted color space explicitly via cv2's built-in transformation targeting BGR format. Alignment locked in just before detection ran.
Got caught in a Git snag when trying to send updates to GitHub. A fresh fetch revealed outside edits blocking the upload. Pulling those down dropped us straight into Vim, waiting on a message we did not want to write. Stuck inside that screen felt confusing at first. Hitting escape then typing :q! didn’t help - it kept returning. We found out later that git merge --abort was the real exit key. After stepping back safely, we added --no-edit next time so Git handled the note itself. That small tweak let everything move forward without another pause. The fix now feels obvious, though it took some frustration to get there.

🎯 Conclusion
This lab tied everyday Python AI methods to a sharp 3D sim - done right, not just fast. Through tight control of the rendering cycle, real-time third-eye object monitoring emerged while keeping CARLA smooth and responsive.
Later upgrades could try performance checks. Instead of Nano, using YOLOv8 Small might show clearer speed costs. Each test would track how much accuracy improves. Speed loss would appear beside those gains. The trade-off becomes visible through direct comparison. Frame rates shift when swapping models. What’s lost in speed may be gained in precision. Testing reveals real differences, not estimates.
Out here, guessing how far away things are starts with pulling box spots from the scene - this sets up a crash-braking trick later on. One thing leads to another when the car begins spotting obstacles and sizing up gaps. Step one? Grab those frame edges so the system can judge spacing. Distance guesses kick off once the model marks where each object sits. The vehicle needs space clues before it reacts. Box lines come first, then decisions follow. From edge numbers, reactions grow. Spotting range means starting with shape borders drawn around what shows up.

📎 Appendix
Place yolov8n.pt inside the /model/ folder, sitting alongside manual_control.py. The file needs to be right there, no detours. Its home is that directory - nowhere else. Right where the script expects it, waiting. Not above, not below, but in it. That spot matters more than it seems at first glance.
A picture can go right there if you feel like showing off how the simulator looks while it runs. Just drag it into the GitHub readme spot made for images.

📚 References
Ultralytics YOLOv8 Python Guide
CARLA Simulator Python API Documentation
GE Practical SS26 IT Module Exercise 4 Guidelines Lab PDF
