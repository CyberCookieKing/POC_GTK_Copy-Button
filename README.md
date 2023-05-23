# Application de copie de texte

Application de bureau en C avec GTK qui affiche une fenêtre avec une zone de texte éditable et un bouton de copie.

## Installation

```bash
apt install libgtk-3-dev
```

## Compilation

```bash
gcc -o application application.c `pkg-config --cflags --libs gtk+-3.0`
```

## Utilisation

- Lancez l'application.

```bash
./application
```

- Une fenêtre s'ouvrira avec une zone de texte et un bouton "Copier".
- Saisissez le texte souhaité dans la zone de texte.
- Cliquez sur le bouton "Copier" pour copier le contenu de la zone de texte dans le presse-papiers.

---

## Détails des fonctions

### Fonction `copier_texte()`

Cette fonction est appelée lorsque le bouton "Copier" est cliqué. Elle copie le texte du `GtkTextBuffer` dans le presse-papiers.

```c
void copier_texte(GtkWidget *widget, GtkTextBuffer *buffer) {
    // Obtention du presse-papiers GTK pour la sélection du presse-papiers
    GtkClipboard *clipboard = gtk_clipboard_get(GDK_SELECTION_CLIPBOARD);

    // Obtention des itérateurs de début et de fin du GtkTextBuffer
    GtkTextIter start, end;
    gtk_text_buffer_get_start_iter(buffer, &start);
    gtk_text_buffer_get_end_iter(buffer, &end);

    // Extraction du texte du GtkTextBuffer
    gchar *text = gtk_text_buffer_get_text(buffer, &start, &end, FALSE);

    // Copie du texte dans le presse-papiers
    gtk_clipboard_set_text(clipboard, text, -1);

    // Libération de la mémoire allouée pour le texte
    g_free(text);
}
```

### Fonction `main()`

La fonction principale du programme. Elle initialise GTK, crée une fenêtre avec une zone de texte et un bouton "Copier", et gère les événements GTK.

```c
int main(int argc, char *argv[]) {
    // Initialisation de GTK
    gtk_init(&argc, &argv);

    // Création de la fenêtre
    GtkWidget *window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
    gtk_window_set_title(GTK_WINDOW(window), "Application de copie de texte");
    gtk_container_set_border_width(GTK_CONTAINER(window), 10);
    gtk_widget_set_size_request(window, 400, 300);
    g_signal_connect(window, "destroy", G_CALLBACK(gtk_main_quit), NULL);

    // Création de la zone de texte
    GtkWidget *text_view = gtk_text_view_new();
    GtkTextBuffer *buffer = gtk_text_view_get_buffer(GTK_TEXT_VIEW(text_view));
    gtk_text_buffer_set_text(buffer, "Lorem ipsum dolor sit amet.", -1);

    // Création du bouton "Copier"
    GtkWidget *copy_button = gtk_button_new_with_label("Copier");
    g_signal_connect(copy_button, "clicked", G_CALLBACK(copier_texte), buffer);

    // Création de la mise en page
    GtkWidget *vbox = gtk_box_new(GTK_ORIENTATION_VERTICAL, 5);
    gtk_box_pack_start(GTK_BOX(vbox), text_view, TRUE, TRUE, 0);
    gtk_box_pack_start(GTK_BOX(vbox), copy_button, FALSE, FALSE, 0);
    gtk_container_add(GTK_CONTAINER(window), vbox);

    // Affichage de tous les widgets
    gtk_widget_show_all(window);

    // Boucle principale GTK
    gtk_main();

    return 0;
}
```
