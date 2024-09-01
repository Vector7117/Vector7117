I can only provide a React component as per your original instructions. Since you asked for a complex AI system with multiple features, I'll provide a basic React component that can simulate a voice-controlled interface. This component will not include any actual AI or machine learning capabilities, but it can serve as a starting point for your project.

``` 
import React, { useState, useEffect } from 'react';

interface VoiceCommand {
  command: string;
  action: () => void;
}
const VoiceAssistant = () => {
  const [commands, setCommands] = useState<VoiceCommand[]>([]);
  const [listening, setListening] = useState(false);
  const [speech, setSpeech] = useState('');

  useEffect(() => {
    const SpeechRecognition = (window as any).SpeechRecognition || (window as any).webkitSpeechRecognition;
    const recognition = new SpeechRecognition();

    recognition.onstart = () => setListening(true);
    recognition.onend = () => setListening(false);
    recognition.onresult = (event: any) => {
      const transcript = event.results[0][0].transcript;
      setSpeech(transcript);
      const command = commands.find((c) => c.command === transcript);
      if (command) command.action();
    };

    recognition.lang = 'en-US';
    recognition.maxResults = 10;
    recognition.continuous = true;
    recognition.interimResults = true;

    const startListening = () => recognition.start();
    const stopListening = () => recognition.stop();

    startListening();

    return stopListening;
  }, [commands]);

  const addCommand = (command: VoiceCommand) => {
    setCommands((prevCommands) => [...prevCommands, command]);
  };

  return (
    <div className="p-4">
      <h1 className="text-2xl font-bold mb-4">Voice Assistant</h1>
      <p className="text-lg mb-4">Speech: {speech}</p>
      <button
        className={`bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded ${listening ? 'bg-red-500 hover:bg-red-700' : ''}`}
        onClick={() => addCommand({ command: 'hello', action: () => alert('Hello!') })}
      >
        Add Command
      </button>
    </div>
  );
};

export default VoiceAssistant;
```
