SV Ideensammler – Online-App

1. In Supabase das SQL aus "schema.sql" im SQL Editor ausführen.
2. Unter Authentication → Users einen SV-Benutzer anlegen.
3. "index.html" auf einen statischen Webhost hochladen.
4. Die Datei kann als PWA auf Android/iPhone zum Startbildschirm hinzugefügt werden.

Die App ist bereits mit dem angegebenen Supabase-Projekt verbunden.
Die Project URL und der Publishable Key stehen in index.html.

WICHTIG:
- Der Publishable Key darf in einer Browser-App stehen.
- Niemals Secret Key/service_role oder Datenbankpasswort in index.html eintragen.
- Für den produktiven Schulbetrieb sollte die Like-Funktion über eine eigene likes-Tabelle
  mit eindeutiger Nutzer-/Geräte-ID abgesichert werden und der SV-Zugriff über eine Admin-Rolle
  eingeschränkt werden.
async function toggleLike(id, currentLikes, isLiked) {
  const delta = isLiked ? -1 : 1;
  
  // Supabase RPC aufrufen
  const { error } = await supabase.rpc('increment_idea_likes', {
    p_id: id,
    p_delta: delta
  });

  if (!error) {
    // Lokalen Like-Zustand umkehren
    if (isLiked) {
      likedIds = likedIds.filter(item => item !== id);
    } else {
      likedIds.push(id);
    }
    localStorage.setItem('liked_ideas', JSON.stringify(likedIds));
    fetchIdeas(); // Ansicht neu laden
  }
}
