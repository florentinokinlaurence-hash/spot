import java.util.*;

public class PlaylistManager {
    private List<String> playlist = new ArrayList<>();
    private Deque<List<String>> undoStack = new ArrayDeque<>();
    private Deque<List<String>> redoStack = new ArrayDeque<>();
    private Scanner scanner = new Scanner(System.in);

    public void start() {
        while (true) {
            displayMenu();
            String choice = scanner.nextLine().trim();
            switch (choice) {
                case "1": addSong(); break;
                case "2": removeLastSong(); break;
                case "3": undo(); break;
                case "4": redo(); break;
                case "5": viewPlaylist(); break;
                case "6": System.out.println("Exiting..."); return;
                default: System.out.println("Invalid choice. Try again.");
            }
        }
    }

    private void displayMenu() {
        System.out.println("\nMenu Options:");
        System.out.println("1. Add song");
        System.out.println("2. Remove last song");
        System.out.println("3. Undo");
        System.out.println("4. Redo");
        System.out.println("5. View playlist");
        System.out.println("6. Exit");
        System.out.print("Select an option: ");
    }

    private void addSong() {
        System.out.print("Enter song name: ");
        String song = scanner.nextLine().trim();
        pushUndoState();
        redoStack.clear();
        playlist.add(song);
        System.out.println("Added: " + song);
    }

    private void removeLastSong() {
        if (playlist.isEmpty()) {
            System.out.println("Playlist is empty. Nothing to remove.");
            return;
        }
        pushUndoState();
        redoStack.clear();
        String removed = playlist.remove(playlist.size() - 1);
        System.out.println("Removed: " + removed);
    }

    private void undo() {
        if (undoStack.isEmpty()) {
            System.out.println("Nothing to undo.");
            return;
        }
        redoStack.push(copyPlaylist(playlist));
        playlist = undoStack.pop();
        System.out.println("Undo performed.");
    }

    private void redo() {
        if (redoStack.isEmpty()) {
            System.out.println("Nothing to redo.");
            return;
        }
        undoStack.push(copyPlaylist(playlist));
        playlist = redoStack.pop();
        System.out.println("Redo performed.");
    }

    private void viewPlaylist() {
        if (playlist.isEmpty()) {
            System.out.println("(empty)");
        } else {
            System.out.println("Current playlist:");
            for (int i = 0; i < playlist.size(); i++) {
                System.out.printf("%d. %s%n", i + 1, playlist.get(i));
            }
        }
    }

    private void pushUndoState() {
        undoStack.push(copyPlaylist(playlist));
    }

    private List<String> copyPlaylist(List<String> src) {
        return new ArrayList<>(src);
    }

    public static void main(String[] args) {
        new PlaylistManager().start();
    }
}


























































































