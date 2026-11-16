# Drone Adaptive PID Control with RBF Network

This project simulates flight control for a quadcopter drone by combining a traditional PID controller with a Radial Basis Function (RBF) Neural Network to dynamically optimize the $Kp, Ki, Kd$ gains in real time.

## 1. System Requirements & Installed Libraries

To run this project, you need **Python 3.x** and the following third-party libraries:

* **NumPy**: Used for matrix operations, RBF network computations, and physics simulation.
* **Matplotlib**: Used for real-time visualization of the drone's flight states and motor outputs.

*(Note: Standard Python libraries such as `random`, `time`, `math`, and `os` are also used and come pre-installed with Python).*

### Quick Library Installation

Open your Terminal or Command Prompt and run:

```bash
pip install numpy matplotlib
```

## 2. Project Structure

Ensure all three core files are placed in the same directory:

1. **`pid.py`**: Defines the `PID` controller class, the `DroneSimulator` physics engine (including wind disturbance, lift, and inertia), the motor `mixer`, and the main simulation step function (`run`).
2. **`Evolution.py`**: Implements the `RBFNetwork` using reinforcement learning to train and optimize the PID parameters, saving the best-performing weights as `.npy` files.
3. **`fly.py`**: Loads the trained AI model, prompts the user for target flight setpoints (Altitude, Pitch, Roll, Yaw), and launches a real-time interactive Matplotlib dashboard to monitor the drone's flight.

## 3. Step-by-Step Instructions to Run

### Step 1: Train the AI Model
Before running the visual flight simulation, you must train the RBF network so it can generate the required weight files.

Run the following command in your project directory:

```bash
python Evolution.py
```

* **What happens during this step?** The script runs 1,000 episodes of randomized flight scenarios to maximize the reward function. Whenever a better reward is achieved, it automatically saves four weight files into the project directory:
  * `best_centers.npy`
  * `best_betas.npy`
  * `best_W2.npy`
  * `best_b2.npy`

*(Note: You do not need to wait for all 1,000 episodes to finish if you just want to test the simulation—as soon as the four `.npy` files appear in your folder, you can stop the training with `Ctrl + C` and proceed to Step 2).*

### Step 2: Run the Real-Time Flight Simulation
Once the `.npy` weight files are present in your directory, launch the flight simulator:

```bash
python fly.py
```

The terminal will prompt you to enter the target setpoints for the drone:

1. **Altitude (m)**: Target height (e.g., `5.0`)
2. **Pitch (°)**: Target pitch angle (e.g., `0.0`)
3. **Roll (°)**: Target roll angle (e.g., `0.0`)
4. **Yaw (°)**: Target heading angle (e.g., `90.0`)

### Step 3: Monitor and Interact with the Results
* A Matplotlib window will open displaying 5 real-time subplots:
  1. **Altitude** vs. Setpoint ($m$)
  2. **Pitch** vs. Setpoint ($^\circ$)
  3. **Roll** vs. Setpoint ($^\circ$)
  4. **Yaw** vs. Setpoint ($^\circ$)
  5. **Motor Speeds** ($\%$) for all four rotors ($M_{UR}, M_{UL}, M_{DR}, M_{DL}$)
* **Interactive Hover:** You can hover your mouse cursor over any of the graphs to view a crosshair and a tooltip showing exact values at any given timestamp $t$.

### Optional: Test Pure PID Without AI
If you want to test the physics engine using fixed PID gains without the neural network, you can run `pid.py` directly:

```bash
python pid.py
```
Enter the 5 required inputs one per line (`set_alt`, `set_pitch`, `set_roll`, `set_yaw`, and `loop` count) to view the console telemetry output.