// ==UserScript==
// @name         HLTV Achivement Adder
// @match        https://www.hltv.org/*
// @grant        none
// @author       Sarpmanon
// @version      1.0
// @run-at       document-end
// ==/UserScript==

(function() {
if (document.readyState === "loading") {
    document.addEventListener("DOMContentLoaded", doStuff);
} else {
    doStuff();
}

function doStuff() {
    try {
        //Where the HLTV logo sits
        const socialMediaButtons = document.querySelector(".socialMediaButtons")
        const addButton = document.createElement("a");

        const trophyImageSrc = "https://img-cdn.hltv.org/eventtrophy/sRnGzqT9Rqj-uJMatfEqYo.png?ixlib=java-2.1.0&w=200&s=ac4f9d4f05ce4d476b5bcac6682f6023";

        addButton.id = "addTrophyButton";
        addButton.style = "bold";
        addButton.style.background = "black";
        addButton.style.width = "32px";
        addButton.style.height = "32px";
        addButton.style.justifyContent = "center"
        addButton.style.fontSize = "30px";
        addButton.style.borderRadius = "4px"
        addButton.style.background = `#3870a3 url(${trophyImageSrc}) center / 24px 24px no-repeat`;
        addButton.style.boxShadow = "inset 0 1px 0 0 hsla(0,0%,100%,.06),0 1px 2px 0 rgba(0,0,0,.46)";
        addButton.style.boxShadow = "0 1px 1px 0 rgba(0,0,0,.5)"

        socialMediaButtons.appendChild(addButton);

        //Event listeners
        addButton.addEventListener("click", doMainStuff)
        addButton.addEventListener("mouseenter", () => {
            addButton.style.backgroundColor = "#2d5b85"
        })
        addButton.addEventListener("mouseleave", () => {
            addButton.style.backgroundColor = "#3870a3"
        })
    } catch (error) {
        console.log("Something went wrong:", error.toString())
    }
}

async function doMainStuff() { //This is horrible to look at
    const response = prompt("1: Logo (event URL)\n2: Trophy (trophy URL)");
    const responseOrder = prompt("Enter the order you want the logo to be added\n1: To the left\n2: To the right")
    if (responseOrder != "1" && responseOrder != "2") {
        alert("Invalid option selected.")
        return;
    }
    if (response == "1") {
        const eventURL = prompt("Enter the URL of the event that you wanna add the logo of");
        const result = await fetchImage(eventURL)

        addTrophy(result, responseOrder)
    } else if (response == "2") {
        const eventTitle = prompt("Enter the title of the event.")
        const trophyURL = prompt("Enter the URL of the trophy.")
        if (!eventTitle || !trophyURL) return;

        const result = {
            img: trophyURL,
            title: eventTitle,
        }

        addTrophy(result, responseOrder)
    } else {
        alert("Invalid option selected.")
    }
}

function addTrophy(result, order) {
    const imagesrc = result.img;
    const title = result.title;

    //Trophy row
    const trophyrow = document.querySelector(".trophyRow")
    if (!trophyrow) { //I might make it possible in the future for the script to create the trophyrow if not found
        alert("This player hasn't even won real single trophy yet. Thus fake ones cannot be added.");
        return;
    }

    //Adding the trophy
    console.log("Creating the trophy...")
    const trophy = document.createElement("a");
    trophy.classList.add("trophy")
    const trophyHolderDiv = document.createElement("div")
    trophyHolderDiv.classList.add("trophyHolder")
    const trophyDescriptionSpan = document.createElement("span")
    trophyDescriptionSpan.classList.add("trophyDescription")
    trophyDescriptionSpan.title = title.toString().trim();
    const trophyImg = document.createElement("img")
    trophyImg.src = imagesrc;
    trophyImg.classList.add("trophyIcon")

    //Getting everything together
    console.log("Getting everything together...")
    console.log("Adding the trophy...")
    trophyDescriptionSpan.appendChild(trophyImg)
    trophyHolderDiv.appendChild(trophyDescriptionSpan)
    trophy.appendChild(trophyHolderDiv)
    trophyrow.appendChild(trophy)

    let allTrophies = document.querySelectorAll(".trophy");
    var mvpmedal = null;
    allTrophies.forEach(singletrophy => {
        const trophyHolderChild = singletrophy.firstElementChild;
        const trophyDescription = trophyHolderChild.firstElementChild;

        if (trophyDescription.title.toString().trim().includes("MVP winner at")) {
            mvpmedal = singletrophy;
        }
    })

    //Checks the placement order that the user wants
    if (order == "1") trophyrow.prepend(trophy);
    console.log(mvpmedal)
    if (mvpmedal) trophyrow.prepend(mvpmedal);

    console.log("Done!")
}

async function fetchImage(eventURL) {
    //Fetches the site
    console.log("Fetching the URL...")
    const res = await fetch(eventURL);
    const html = await res.text();

    //Gets the image and the title
    console.log("Getting the image...")
    const doc = new DOMParser().parseFromString(html, "text/html");
    const img = doc.querySelector('meta[property="og:image"]')?.content;
    const title = doc.querySelector('meta[property="og:title"]')?.content;

    if (img && title) {
        console.log("Received the metadata!");
        return {img: img, title: title};
    } else {
        console.log("Couldn't get the metadata.");
        return null;
    }
}
})();
