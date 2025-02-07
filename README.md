<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Fake Flight Ticket Generator</title>
    <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="flex flex-col items-center p-6">
    <h1 class="text-2xl font-bold mb-4">Fake Flight Ticket Generator</h1>
    <div id="root"></div>
    <script type="module">
        import React, { useState } from "https://cdn.skypack.dev/react";
        import { createRoot } from "https://cdn.skypack.dev/react-dom/client";
        import QRCode from "https://cdn.skypack.dev/qrcode.react";

        function FlightTicketGenerator() {
            const [name, setName] = useState("");
            const [flightNumber, setFlightNumber] = useState("");
            const [airline, setAirline] = useState("");
            const [date, setDate] = useState("");
            const [seat, setSeat] = useState("");
            const [pnr, setPnr] = useState(() => Math.random().toString(36).substring(2, 8).toUpperCase());

            const downloadTicket = () => {
                const ticket = document.getElementById("ticket");
                html2canvas(ticket).then((canvas) => {
                    const link = document.createElement("a");
                    link.href = canvas.toDataURL("image/png");
                    link.download = "flight_ticket.png";
                    link.click();
                });
            };

            return (
                <div className="flex flex-col items-center p-6">
                    <div className="grid grid-cols-2 gap-4 mb-6">
                        <input className="border p-2" placeholder="Passenger Name" value={name} onChange={(e) => setName(e.target.value)} />
                        <input className="border p-2" placeholder="Flight Number" value={flightNumber} onChange={(e) => setFlightNumber(e.target.value)} />
                        <input className="border p-2" placeholder="Airline" value={airline} onChange={(e) => setAirline(e.target.value)} />
                        <input className="border p-2" type="date" value={date} onChange={(e) => setDate(e.target.value)} />
                        <input className="border p-2" placeholder="Seat Number" value={seat} onChange={(e) => setSeat(e.target.value)} />
                    </div>
                    <div id="ticket" className="p-4 text-center border rounded-xl shadow-lg bg-white w-96">
                        <h2 className="text-xl font-semibold">{airline || "Airline"}</h2>
                        <p className="text-gray-700">Passenger: {name || "John Doe"}</p>
                        <p>Flight: {flightNumber || "XX123"} | Seat: {seat || "12A"}</p>
                        <p>Date: {date || "2025-02-07"}</p>
                        <p>PNR: {pnr}</p>
                        <div className="flex justify-center mt-4">
                            <QRCode value={`${name}-${flightNumber}-${pnr}`} size={80} />
                        </div>
                    </div>
                    <button className="mt-4 bg-blue-500 text-white px-4 py-2 rounded" onClick={downloadTicket}>Download Ticket</button>
                </div>
            );
        }

        createRoot(document.getElementById("root")).render(<FlightTicketGenerator />);
    </script>
</body>
</html>
