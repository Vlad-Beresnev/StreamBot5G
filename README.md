# Solution for Ultrahack Unbreakable Connectivity Hackathon
![alt text](./assets/image-3.png)

## Team Members
- **Syed Jawad Akhtar**: [LinkedIn Profile](https://www.linkedin.com/in/syed-jawad-akhtar/)
- **Reem Suleiman**: [LinkedIn Profile](https://www.linkedin.com/in/reem-suleiman/)
- **Muhammad Omer Amjad**: [Linkedin Profile](https://www.linkedin.com/in/omeramjad/)
- **Vlad Beresnev**: [LinkedIn Profile](https://www.linkedin.com/in/vlad-beresnev-5b6673269)
- **David Vasquez**: [LinkedIn Profile](https://www.linkedin.com/in/baskelos/)
- **Sergei Kleimenov**: [LinkedIn Profile](https://www.linkedin.com/in/sergei-kleimenov-13937419/)
- **Juha Kela**: [LinkedIn Profile](https://www.linkedin.com/in/juhakela/)

# Live Video Stream using NaC & 5G
## Overview
This project demonstrates a live video streaming application using Network as Code (NaC) and 5G technology. The project is divided into two main components: the frontend and the backend.

## Project Structure
- `./frontend/src/App.jsx`: The frontend component built with React.
- `./backend/cv2Server.py`: The backend component built with Flask and OpenCV.

## Frontend: ./frontend/src/app.jsx
The frontend is responsible for displaying the live video stream and providing user interaction options.

### Key Features
- Displays live video stream from the backend.
![alt text](./assets/image.png)
- Provides a "QoD Video Boost" button to enhance video quality.
![alt text](./assets/image-1.png)
- Toggles between "QoD Video Boost" and "Stop QoD Session" buttons based on user interaction.
![alt text](./assets/image-2.png)

## Backend: ./backend/cv2Server.py
The backend is responsible for capturing video from the camera, processing it using OpenCV, and serving it to the frontend via a Flask web server.

### Key Features
- Captures video frames from the camera using OpenCV.
    ```python
    def generate_frames():
        cap = cv2.VideoCapture(0)  # Open the default webcam
        while True:
            success, frame = cap.read()  # Capture frame-by-frame
            if not success:
                break

            # Encode the frame in JPEG format
            ret, buffer = cv2.imencode('.jpg', frame)
            if not ret:
                continue

            # Convert the buffer to bytes and yield it
            stream = io.BytesIO(buffer)
            stream.seek(0)
            yield (b'--frame\r\n'
                b'Content-Type: image/jpeg\r\n\r\n' + stream.read() + b'\r\n')
            stream.truncate()
        cap.release()  # Release the webcam

- Streams the video frames to the frontend using Flask.
    ```python
    @app.route('/video_feed', methods=['GET'])
    def video_feed():
        frames_response = Response(generate_frames(), mimetype='multipart/x-mixed-replace; boundary=frame')
        return frames_response

- Applies a Quality of Service (QoS) profile to optimize for low latency using NaC (Network as Code) by Nokia.
![alt text](./assets/image-4.png)

### Endpoints
- `/video_feed`: Streams the live video feed to the frontend.

### Dependencies
- Flask: A lightweight WSGI web application framework.
- OpenCV: A library for computer vision tasks.
- NaC: Network as Code for managing network resources.

### How to Run
1. Install the required dependencies:
   ```bash
   pip install flask opencv-python network_as_code