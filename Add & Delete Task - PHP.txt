<?php
// File where tasks are stored
$file = "tasks.txt";

// Handle task addition
if ($_SERVER["REQUEST_METHOD"] === "POST" && isset($_POST["task"])) {
    $task = trim($_POST["task"]);
    if (!empty($task)) {
        file_put_contents($file, $task . PHP_EOL, FILE_APPEND);
    }
    header("Location: " . $_SERVER['PHP_SELF']);
    exit;
}

// Handle task deletion
if (isset($_GET['delete'])) {
    $id = (int)$_GET['delete']; // Convert to integer for safety

    // Read all tasks
    $tasks = file_exists($file) ? file($file, FILE_IGNORE_NEW_LINES) : [];

    // Remove the selected task
    if (isset($tasks[$id])) {
        unset($tasks[$id]);
        file_put_contents($file, implode(PHP_EOL, $tasks) . PHP_EOL);
    }

header("Location: " . $_SERVER['PHP_SELF']);
    exit;
}

// Read tasks
$tasks = file_exists($file) ? file($file, FILE_IGNORE_NEW_LINES) : [];
?>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>To-Do List</title>
</head>
<body>
    <h1>Simple To-Do List</h1>

    <form method="POST" action="">
        <input type="text" name="task" placeholder="Enter a task" required>
        <button type="submit">Add Task</button>
    </form>

 <h2>Tasks:</h2>
    <ul>
        <?php foreach ($tasks as $index => $task): ?>
            <li>
                <?= htmlspecialchars($task); ?>
                <a href="<?= $_SERVER['PHP_SELF']; ?>?delete=<?= $index; ?>">Delete</a>
            </li>
        <?php endforeach; ?>
    </ul>
</body>
</html>
