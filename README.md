# streaming-video
streaming video streaming project using spring reactive web app 

```markdown
# Reactive Spring Video Streaming Application

This project demonstrates a video streaming service built using **Spring Boot WebFlux** (reactive programming). It allows users to stream video files stored in the `src/main/resources/videos` directory.

---

## Features

- **Video Streaming**: Stream video files directly in the browser.
- **Reactive Programming**: Built with Spring Boot WebFlux for non-blocking, efficient handling of requests.
- **REST API**: Fetch video content dynamically using an API endpoint.
- **Bootstrap UI**: A simple front-end interface styled with Bootstrap.

---

## Project Structure

```
src/main/resources/videos/
├── DevOps.mp4
src/main/java/
├── service/
│   ├── StreamingService.java
├── controller/
│   ├── SpringwebfluxVideoStreamingApplication.java
```

---

## Application Workflow

1. **Frontend**:
   - The HTML page (`index.html`) provides a video player that requests video files dynamically via the API.
   - Users can play videos with basic controls like play, pause, and seek.

2. **Backend**:
   - The `StreamingService` class handles loading video files as `Resource` objects.
   - The REST controller (`SpringwebfluxVideoStreamingApplication`) manages API requests and streams video data in chunks.

---

## Code Explanation

### Frontend (HTML)

The UI for the application is simple and uses **Bootstrap** for styling.

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.0/dist/css/bootstrap.min.css" rel="stylesheet">
    <title>Reactive Spring Video Streaming</title>
</head>
<body>
    <div class="container mt-5">
        <h2>Video Streaming</h2>
        <video src="videos/DevOps" width="360px" height="380px" controls preload="none"></video>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.0/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>
```

**Key Elements**:
- **Video Player**: Uses the `<video>` tag with `controls` for basic playback features.
- **Bootstrap**: Provides a responsive layout.

---

### Backend

#### **StreamingService**

The service retrieves video resources from the file system using the `ResourceLoader`.

```java
@Service
public class StreamingService {

    private static final String FORMAT = "classpath:videos/%s.mp4";

    @Autowired
    private ResourceLoader resourceLoader;

    public Mono<Resource> getVideo(String title) {
        return Mono.fromSupplier(() -> resourceLoader.getResource(String.format(FORMAT, title)));
    }
}
```

**Key Highlights**:
- **Reactive API**: The `Mono` type enables non-blocking processing.
- **Dynamic Path**: Videos are dynamically loaded based on the provided `title`.

---

#### **SpringwebfluxVideoStreamingApplication**

The controller exposes a REST API endpoint to stream videos.

```java
@RestController
public class SpringwebfluxVideoStreamingApplication {

    @Autowired
    private StreamingService service;

    @GetMapping(value = "videos/{title}", produces = "video/mp4")
    public Mono<Resource> getVideos(@PathVariable String title, @RequestHeader("Range") String range) {
        System.out.println("Range in bytes: " + range);
        return service.getVideo(title);
    }
}
```

**Key Highlights**:
- **Dynamic Endpoint**: Serves videos from `/videos/{title}`.
- **Byte Range**: Handles range requests for smooth streaming.

---

## Getting Started

### Prerequisites

- **Java 17** or higher
- **Maven** (for building the project)

### Running the Application

1. Clone the repository:
   ```bash
   git clone <repository-url>
   cd <repository-directory>
   ```

2. Add video files to `src/main/resources/videos`.

3. Start the application:
   ```bash
   mvn spring-boot:run
   ```

4. Open the application in your browser at `http://localhost:8080`.

---

## Example API Request

To fetch a video:

```bash
GET http://localhost:8080/videos/DevOps
Headers:
  Range: bytes=0-
```

---

## Technologies Used

- **Spring Boot WebFlux**: Reactive backend framework.
- **Bootstrap**: Responsive CSS framework.
- **Java**: Backend programming language.

---

## Screenshots

### Video Streaming Page
![Screenshot of the Video Streaming Page](screenshot.png)

---

