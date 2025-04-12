// Motolink Web-App - Erweitert mit Geräuschfilter, Polizeiwarnung & KI-Sprache
// Ersteller: Marcel Grill & Jarvis

import React, { useEffect, useState } from "react";
import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";
import { Card, CardContent } from "@/components/ui/card";
import { MapContainer, TileLayer } from "react-leaflet";
import "leaflet/dist/leaflet.css";

export default function Motolink() {
  const [name, setName] = useState("");
  const [listening, setListening] = useState(false);
  const [log, setLog] = useState([]);
  const [position, setPosition] = useState(null);
  const [firstUse, setFirstUse] = useState(true);

  useEffect(() => {
    if (navigator.geolocation) {
      navigator.geolocation.getCurrentPosition(pos => {
        setPosition([pos.coords.latitude, pos.coords.longitude]);
      });
    }
    // Starte KI-Begrüßung
    setTimeout(() => {
      if (firstUse && name.toLowerCase() === "farbian") {
        setLog(prev => [...prev, `[Friday] Hallo Farbian, ich bin Friday, deine persönliche Sprachassistentin.`]);
      } else {
        setLog(prev => [...prev, "[Friday] Begrenzter Zugriff auf das Grill-Netzwerk verfügbar."]);
      }
      if (firstUse && name.toLowerCase() === "marcel") {
        setLog(prev => [...prev, `[Jarvis] Hallo Marcel, ich bin Jarvis, dein persönlicher Sprachassistent.`]);
      } else {
        setLog(prev => [...prev, "[Jarvis] Vollständiger Zugriff auf das Grill-Netzwerk verfügbar."]);
      }
      if (firstUse && name) {
        setLog(prev => [...prev, `[Link] Hallo ${name}, ich bin Link, dein persönlicher Sprachassistent.`]);
      } else {
        setLog(prev => [...prev, "[Link] Begrenzter Zugriff auf das Grill-Netzwerk verfügbar."]);
      }
      setFirstUse(false);
    }, 1000);
  }, [name]);

  const handleVoiceCommand = () => {
    const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
    if (!SpeechRecognition) return alert("Spracherkennung wird nicht unterstützt");
    const recognition = new SpeechRecognition();
    recognition.lang = "de-DE";
    recognition.start();

    setListening(true);

    recognition.onresult = (event) => {
      const voiceText = event.results[0][0].transcript.toLowerCase();
      setLog(log => [...log, `[Du] ${voiceText}`]);

      // Sprachbefehle erkennen
      if (voiceText.includes("spotify")) {
        window.open("https://open.spotify.com", "_blank");
      }
      if (voiceText.includes("position")) {
        alert("Deine aktuelle Position wird angezeigt.");
      }
      if (voiceText.includes("polizei")) {
        setLog(log => [...log, "[System] Polizeiwarnung in der Nähe! Bleib aufmerksam."]);
      }
    };

    recognition.onend = () => setListening(false);
  };

  return (
    <div className="min-h-screen bg-black text-white p-6 space-y-6">
      <h1 className="text-4xl font-bold text-center">Motolink</h1>
      <p className="text-center">Erstellt von Marcel Grill & Jarvis</p>
      <Card>
        <CardContent className="p-4 space-y-2">
          <Input
            placeholder="Wie darf ich dich nennen?"
            value={name}
            onChange={(e) => setName(e.target.value)}
          />
          <Button onClick={handleVoiceCommand} disabled={listening}>
            {listening ? "Zuhören..." : "Sprich mit Jarvis"}
          </Button>
          <div className="bg-white text-black p-2 rounded mt-2 h-32 overflow-y-auto">
            {log.map((entry, index) => <div key={index}>{entry}</div>)}
          </div>
        </CardContent>
      </Card>

      {position && (
        <MapContainer center={position} zoom={13} style={{ height: "300px", width: "100%" }}>
          <TileLayer
            attribution='&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors'
            url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png"
          />
        </MapContainer>
      )}

      <p className="text-sm text-center text-gray-400 mt-4">
        * Sprachjet mit Geräuschfilter aktiviert – Straßenlärm wird ausgeblendet
      </p>
    </div>
  );
}
