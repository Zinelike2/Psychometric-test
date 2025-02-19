import React, { useState } from "react";
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";

const questions = [
  {
    question: "Which activity do you enjoy the most?",
    options: [
      "Solving mathematical problems",
      "Designing and creating things",
      "Conducting experiments and researching",
      "Leading a group or managing a project",
      "Helping others solve their problems"
    ],
    category: "Logic"
  },
  {
    question: "What kind of books or articles do you prefer to read?",
    options: [
      "Science and technology",
      "Business and economics",
      "Art and design",
      "Psychology and human behavior",
      "Health and medicine"
    ],
    category: "Knowledge"
  }
];

export default function PsychometricTest() {
  const [currentQuestion, setCurrentQuestion] = useState(0);
  const [answers, setAnswers] = useState([]);
  const [scores, setScores] = useState({});
  const [result, setResult] = useState(null);

  const handleAnswer = (answer, category) => {
    setAnswers([...answers, answer]);
    setScores((prevScores) => ({
      ...prevScores,
      [category]: (prevScores[category] || 0) + 1
    }));
    
    if (currentQuestion < questions.length - 1) {
      setCurrentQuestion(currentQuestion + 1);
    } else {
      calculateResult();
    }
  };

  const calculateResult = () => {
    const recommendedCareer = Object.keys(scores).reduce((a, b) => scores[a] > scores[b] ? a : b);
    setResult(`Your recommended career path is related to ${recommendedCareer}.`);
  };

  return (
    <div className="flex flex-col items-center justify-center min-h-screen p-4">
      <Card className="w-full max-w-md p-6 text-center">
        <CardContent>
          {result ? (
            <h2 className="text-xl font-bold mb-4">{result}</h2>
          ) : (
            <>
              <h2 className="text-xl font-bold mb-4">{questions[currentQuestion].question}</h2>
              {questions[currentQuestion].options.map((option, index) => (
                <Button 
                  key={index} 
                  onClick={() => handleAnswer(option, questions[currentQuestion].category)} 
                  className="block w-full mb-2">
                  {option}
                </Button>
              ))}
            </>
          )}
        </CardContent>
      </Card>
    </div>
  );
}
