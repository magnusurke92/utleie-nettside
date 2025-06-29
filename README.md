import { useState } from "react";
import { Calendar } from "@shadcn/ui/calendar";
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { format } from "date-fns";

export default function UtleieKalender() {
  const [selectedDates, setSelectedDates] = useState([]);
  const [price, setPrice] = useState(0);

  const calculatePrice = (dates) => {
    if (dates.length === 0) return 0;
    const days = dates.length;
    return 1500 + (days - 1) * 1000;
  };

  const handleDateSelect = (date) => {
    const dStr = format(date, "yyyy-MM-dd");
    let newDates = [...selectedDates];
    if (newDates.includes(dStr)) {
      newDates = newDates.filter((d) => d !== dStr);
    } else {
      newDates.push(dStr);
    }
    newDates.sort();
    setSelectedDates(newDates);
    setPrice(calculatePrice(newDates));
  };

  return (
    <div className="max-w-xl mx-auto mt-10 p-4">
      <h1 className="text-2xl font-bold mb-4 text-center">Utleie av lift og gravemaskin</h1>
      <p className="text-center mb-6">Dag 1: 1500 kr inkl. mva – Følgende dager: 1000 kr</p>
      <Card>
        <CardContent className="p-4">
          <Calendar
            mode="multiple"
            selected={selectedDates.map((d) => new Date(d))}
            onDayClick={handleDateSelect}
            className="mb-4"
          />
          <div className="text-center">
            <p className="text-lg font-semibold">Valgte dager: {selectedDates.length}</p>
            <p className="text-lg">Totalpris: {price} kr</p>
          </div>
          <div className="text-center mt-4">
            <Button className="w-full" onClick={() => alert("Send Vipps til 123456 - beløp: " + price + " kr")}>Betal med Vipps</Button>
          </div>
        </CardContent>
      </Card>
    </div>
  );
}
