import { useState } from "react"; import { Card, CardContent } from "@/components/ui/card"; import { Input } from "@/components/ui/input"; import { Button } from "@/components/ui/button"; import { Textarea } from "@/components/ui/textarea"; import { Calendar } from "@/components/ui/calendar"; import { format } from "date-fns";

export default function PieceByPiecePros() { const [selectedDate, setSelectedDate] = useState<Date | undefined>(); const [time, setTime] = useState(""); const [request, setRequest] = useState(""); const [file, setFile] = useState<File | null>(null); const [emailSent, setEmailSent] = useState(false);

const handleFileChange = (e: React.ChangeEvent<HTMLInputElement>) => { const selected = e.target.files ? e.target.files[0] : null; if (selected && !selected.type.startsWith("image/")) { alert("Only image files are allowed."); e.target.value = ""; return; } setFile(selected); };

const handleSubmit = async () => { if (selectedDate && time && request) { const formData = new FormData(); formData.append("date", format(selectedDate, "PPP")); formData.append("time", time); formData.append("request", request); if (file) { formData.append("file", file); }

try {
    const response = await fetch("https://formspree.io/f/mwkgykeq", {
      method: "POST",
      body: formData,
      headers: {
        Accept: "application/json",
      },
    });

    if (response.ok) {
      setEmailSent(true);
      setTime("");
      setRequest("");
      setSelectedDate(undefined);
      setFile(null);
    } else {
      alert("Failed to send your request. Please try again later.");
    }
  } catch (error) {
    alert("There was an error sending your request.");
    console.error(error);
  }
}

};

return ( <div className="min-h-screen p-8 bg-gray-50"> <div className="text-center mb-8"> <h1 className="text-4xl font-bold">Piece-by-Piece Pros</h1> <p className="text-xl italic mt-2">Piece-by-Piece, so you don't have to.</p> <p className="text-lg mt-2">Professional in-home assembly services for your furniture, grills, patio sets, exercise equipment, and more.</p> </div>

<div className="grid grid-cols-1 md:grid-cols-2 gap-6">
    <Card>
      <CardContent className="p-4">
        <h2 className="text-xl font-semibold mb-2">Top Brands We Work With</h2>
        <div className="grid grid-cols-2 gap-4">
          <img src="/rogue-logo.png" alt="Rogue" className="object-contain h-20" />
          <img src="/traeger-logo.png" alt="Traeger" className="object-contain h-20" />
          <img src="/pitboss-logo.png" alt="Pitboss" className="object-contain h-20" />
          <img src="/nordictrack-logo.png" alt="NordicTrack" className="object-contain h-20" />
        </div>
      </CardContent>
    </Card>

    <Card>
      <CardContent className="p-4">
        <h2 className="text-xl font-semibold mb-2">Request an Appointment</h2>
        <Calendar mode="single" selected={selectedDate} onSelect={setSelectedDate} />
        <Input
          type="time"
          value={time}
          onChange={(e) => setTime(e.target.value)}
          className="mt-4"
          placeholder="Select time"
        />
        <Textarea
          value={request}
          onChange={(e) => setRequest(e.target.value)}
          className="mt-4"
          placeholder="What would you like us to assemble?"
        />
        <Input
          type="file"
          accept="image/*"
          className="mt-4"
          onChange={handleFileChange}
        />
        <Button className="mt-4 w-full" onClick={handleSubmit}>
          Submit Request
        </Button>
        {emailSent && (
          <p className="mt-2 text-green-600">Your request has been received. We'll reach out via email shortly!</p>
        )}
      </CardContent>
    </Card>
  </div>

  <footer className="mt-12 text-center text-sm text-gray-600">
    Contact us at <a href="mailto:PBP-Pros@outlook.com" className="text-blue-600">PBP-Pros@outlook.com</a><br />
    Logo coming soon.
  </footer>
</div>

); }

