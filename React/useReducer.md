```jsx
import { useReducer, useEffect } from 'react';

// مقداری اولیه ایجکت که قراره داخل کامپوننت استفاده بشه
const initialState = {
  questions: [],
  status: 'loading', // => loading / error / ready / active / finished
  index: 0,
  answer: null,
  points: 0,
  secondsRemaining: null,
};

// منطق کلی ایجکت استیت ها (ها) - ورودی اول مقدار قبلی استیت ها هست و ورودی دوم ایجکت دنیایی که برای ما فرستاده شده
function reducer(state, action) {
  switch (action.type) {
    case 'dataReceived':
      return { ...state, questions: action.payload, status: 'ready' };

    case 'dataFailed':
      return { ...state, status: 'error' };

    case 'startQuiz':
      return { ...state, status: 'active', secondsRemaining: state.questions.length * SECONDS_PER_QUESTION };

    case 'newAnswer':
      const activeQuestion = state.questions[state.index]; // برای پیدا کردن سوال فعال
      return {
        ...state,
        answer: action.payload,
        points: action.payload === activeQuestion.correctOption ? state.points + activeQuestion.points : state.points,
      };

    case 'nextQuestion':
      return { ...state, index: state.index + 1, answer: null };

    case 'finishQuestion':
      return { ...state, status: 'finished' };

    case 'tick':
      return {
        ...state,
        secondsRemaining: state.secondsRemaining - 1,
        status: state.secondsRemaining === 0 ? 'finished' : state.status,
      };

    case 'restartQuestion':
      return { ...initialState, status: 'ready', questions: state.questions };

    default:
      throw new Error('What is your action?');
  }
}

export default function App() {
  // تابعی که قراره ارزش برای فرستادن ایجکت دنیا به تابع ردیوسر استفاده کنیم
  const [state, dispatch] = useReducer(reducer, initialState);
  const { questions, status, index, answer, points, secondsRemaining } = state;

  // دی استراکچر کردن ایجکت استیت ها - استیت هایی که قراره اون توی کامپوننت استفاده بشه
  const numQuestions = questions.length;
  const maxPoints = questions.reduce((pre, cur) => pre + cur.points, 0);

  useEffect(() => {
    async function fetchQuestions() {
      try {
        const response = await fetch('http://localhost:8000/questions');

        if (!response.ok) throw new Error('Failed to fetch questions');

        const data = await response.json();

        if (data) dispatch({ type: 'dataReceived', payload: data });
      } catch (err) {
        // کد کردن دسیج و فرستادن دنیا
        dispatch({ type: 'dataFailed' });
      }
    }
    fetchQuestions();
  }, []);

  return <div>Code....</div>;
}
```
