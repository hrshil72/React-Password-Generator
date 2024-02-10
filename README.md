import { useState, useCallback, useEffect, useRef } from "react";

const App = () => {
let [length, setLength] = useState(8);
let [numberAllowed, setNumberAllowed] = useState(false);
let [charAllowed, setCharAllowed] = useState(false);
let [password, setPassword] = useState("");

//useRef Hook
const passwordRef = useRef(null);

const passwordGenerator = useCallback(() => {
let pass = "";
let str = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz";

    if (numberAllowed) str += "0123456789";
    if (charAllowed) str += "!@#$%^&*()";

    for (let i = 1; i <= length; i++) {
      let char = Math.floor(Math.random() * str.length + 1);
      pass += str.charAt(char);
    }

    setPassword(pass);

}, [length, numberAllowed, charAllowed, setPassword]);

const copyPassword = useCallback(() => {
passwordRef.current?.select();
window.navigator.clipboard.writeText(password);
}, [password]);

useEffect(() => {
passwordGenerator();
}, [length, numberAllowed, charAllowed, setPassword]);

return (
<div className="w-full max-w-2xl	 mx-auto shadow-md rounded-lg px-4 py-3 my-8 bg-gray-800 text-orange-500">
<div className="w-full shadow-md rounded-lg  bg-gray-600 flex flex-col items-center px-7 py-2">
<h1 className="text-white text-3xl mb-5">Password Generator</h1>
<div className="flex w-full shadow rounded-lg overflow-hidden mb-4">
<input
            type="text"
            value={password}
            className="outline-none w-full py-2 px-4 rounded-lg mr-4"
            placeholder="Password"
            ref={passwordRef}
            readOnly></input>

          <button
            onClick={copyPassword}
            className="outline-none bg-blue-700 text-white px-3 py-0.5 shrink-0 rounded-lg ">
            copy
          </button>
        </div>
        <div className="flex gap-5 text-xl mb-5">
          <div className="flex gap-x-2">
            <div className="flex items-center gap-x-1">
              <input
                type="range"
                min={6}
                max={100}
                value={length}
                className="cursor-pointer"
                onChange={(e) => {
                  setLength(e.target.value);
                }}
              />
              <label>Length: {length}</label>
            </div>
          </div>
          <div className="flex items-center gap-x-1">
            <input
              type="checkbox"
              defaultChecked={numberAllowed}
              id="numberInput"
              onChange={() => {
                setNumberAllowed((prev) => !prev);
              }}
            />
            <label htmlFor="numberInput">Numbers</label>
          </div>
          <div className="flex items-center gap-x-1">
            <input
              type="checkbox"
              defaultChecked={charAllowed}
              id="characterInput"
              onChange={() => {
                setCharAllowed((prev) => !prev);
              }}
            />
            <label htmlFor="characterInput">Characters</label>
          </div>
        </div>
      </div>
    </div>

);
};

export default App;
