# No-buy-
import { useEffect, useState } from "react";

export default function NoBuyReminder() {
  const [days, setDays] = useState(0);

  // Lade gespeicherte Tage beim Start
  useEffect(() => {
    const saved = localStorage.getItem("nobuy-days");
    if (saved) setDays(parseInt(saved));
  }, []);

  // Push-Benachrichtigung 1x pro Tag
  useEffect(() => {
    Notification.requestPermission().then((permission) => {
      if (permission === "granted") {
        const now = new Date();
        const lastNotified = localStorage.getItem("nobuy-last-notified");
        const today = now.toDateString();

        if (lastNotified !== today) {
          new Notification("Kauf heute nix. Du bist stärker als der Impuls.");
          localStorage.setItem("nobuy-last-notified", today);
        }
      }
    });
  }, []);

  const increment = () => {
    const newCount = days + 1;
    setDays(newCount);
    localStorage.setItem("nobuy-days", newCount.toString());
  };

  const reset = () => {
    setDays(0);
    localStorage.setItem("nobuy-days", "0");
  };

  return (
    <div className="flex flex-col items-center justify-center h-screen p-6 text-center">
      <h1 className="text-3xl font-bold mb-4">No-Buy Reminder</h1>
      <p className="text-xl mb-2">{days} Tage ohne Spontankauf</p>
      <p className="italic mb-6">„Kauf heute nix. Du bist stärker als
