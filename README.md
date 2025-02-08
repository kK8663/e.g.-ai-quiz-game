# e.g.-ai-quiz-game
import React, { useState, useEffect } from "react";
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";
import { motion } from "framer-motion";

export default function AIGame() {
  const [question, setQuestion] = useState("Click 'Start' to begin the AI quiz!");
  const [answer, setAnswer] = useState("");
  const [score, setScore] = useState(0);
  const [userAnswer, setUserAnswer] = useState("");

  useEffect(() => {
    generateQuestion();
  }, []);

  const fetchAIQuestion = async () => {
    const response = await fetch("https://api.example.com/generate-question"); // Replace with real AI API
    const data = await response.json();
    return { q: data.question, a: data.answer };
  };

  const generateQuestion = async () => {
    try {
      const aiQuestion = await fetchAIQuestion();
      setQuestion(aiQuestion.q);
      setAnswer(aiQuestion.a);
    } catch (error) {
      console.error("Error fetching AI question:", error);
      setQuestion("What is 2 + 2?");
      setAnswer("4");
    }
  };

  const checkAnswer = () => {
    if (userAnswer.toLowerCase() === answer.toLowerCase()) {
      setScore(score + 1);
      alert("Correct!");
    } else {
      alert("Wrong answer. Try again!");
    }
    setUserAnswer("");
    generateQuestion();
  };

  return (
    <div className="flex flex-col items-center justify-center min-h-screen bg-gradient-to-br from-blue-500 to-purple-700 p-6 relative text-white">
      <img 
        src="/mnt/data/WhatsApp Image 1946-11-16 at 02.46.26.jpeg" 
        alt="Kshitiz Gupta" 
        className="absolute top-4 left-4 w-20 h-20 rounded-full border-2 border-white shadow-lg" 
      />
      <p className="absolute top-24 left-4 text-sm font-bold">Created by Kshitiz Gupta</p>
      <Card className="p-6 bg-white shadow-xl rounded-2xl w-full max-w-md text-center text-black">
        <h1 className="text-2xl font-bold mb-4">AI Quiz Game</h1>
        <CardContent>
          <motion.p
            className="text-lg mb-4"
            initial={{ opacity: 0 }}
            animate={{ opacity: 1 }}
            transition={{ duration: 0.5 }}
          >
            {question}
          </motion.p>
          <Input
            type="text"
            placeholder="Your answer..."
            value={userAnswer}
            onChange={(e) => setUserAnswer(e.target.value)}
            className="mb-4 p-2 border rounded"
          />
          <Button onClick={checkAnswer} className="bg-blue-500 text-white px-4 py-2 rounded-lg mr-2">
            Submit
          </Button>
          <Button onClick={generateQuestion} className="bg-green-500 text-white px-4 py-2 rounded-lg">
            New Question
          </Button>
          <p className="mt-4 font-bold">Score: {score}</p>
        </CardContent>
      </Card>
    </div>
  );
}
