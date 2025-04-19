<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8" />
  <title>شجرة العائلة - تلاحم بين عدة شجرات</title>
  <script src="https://balkangraph.com/js/latest/FamilyTree.js"></script>
  <style>
    html, body {
      height: 100%;
      margin: 0;
      font-family: "Cairo", sans-serif;
      direction: rtl;
    }
    #tree {
      width: 100%;
      height: 80vh;
    }
    .popup {
      position: fixed;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      background: white;
      border: 1px solid #ccc;
      padding: 20px;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
      display: none;
      z-index: 100;
    }
    .popup input, .popup select {
      margin-bottom: 10px;
    }
  </style>
  <link href="https://fonts.googleapis.com/css2?family=Cairo&display=swap" rel="stylesheet">
</head>
<body>
  <div id="tree"></div>
  
  <!-- Popup to select two people -->
  <div id="popup" class="popup">
    <h3>اختيار شخصين للمقارنة</h3>
    <select id="person1">
      <!-- Dynamic list of people -->
    </select>
    <select id="person2">
      <!-- Dynamic list of people -->
    </select>
    <button onclick="compare()">قارن</button>
    <button onclick="closePopup()">إغلاق</button>
    <div id="relationship"></div>
  </div>
  
  <script>
    const family = new FamilyTree(document.getElementById("tree"), {
      mode: 'light',
      orientation: FamilyTree.orientation.RIGHT,
      nodeBinding: {
        field_0: "name",
        field_1: "birth",
        img_0: "image"
      },
      nodes: [
        // عائلة محمد
        { id: 1, name: "محمد أحمد", birth: "1950", image: "https://cdn.balkan.app/shared/empty-img.svg", spouse: "أمينة محمد" },
        { id: 2, pid: 1, name: "سعيد محمد", birth: "1975", image: "https://cdn.balkan.app/shared/empty-img.svg", spouse: "فاطمة سعيد" },
        { id: 3, pid: 1, name: "ليلى محمد", birth: "1980", image: "https://cdn.balkan.app/shared/empty-img.svg", spouse: "أحمد عادل" },
        { id: 4, pid: 2, name: "خالد سعيد", birth: "2000", image: "https://cdn.balkan.app/shared/empty-img.svg", spouse: "نورا خالد" },

        // عائلة أحمد (عائلة أخرى)
        { id: 5, name: "أحمد عادل", birth: "1975", image: "https://cdn.balkan.app/shared/empty-img.svg", spouse: "ليلى محمد" },
        { id: 6, pid: 5, name: "فاطمة أحمد", birth: "2005", image: "https://cdn.balkan.app/shared/empty-img.svg", spouse: "محمد سامي" },
        { id: 7, pid: 5, name: "محمود أحمد", birth: "2010", image: "https://cdn.balkan.app/shared/empty-img.svg" },
        
        // رابط بين العائلتين من خلال الزوجة والزوج
        { id: 8, pid: 6, name: "أمينة سامي", birth: "2008", image: "https://cdn.balkan.app/shared/empty-img.svg", spouse: "فاطمة أحمد" }
      ],
      onNodeClick: function(node) {
        alert(`اسم الشخص: ${node.name}\nتاريخ الميلاد: ${node.birth}\nالزوج/الزوجة: ${node.spouse}`);
      }
    });

    // Function to open the comparison popup
    function openPopup() {
      const popup = document.getElementById("popup");
      const person1Select = document.getElementById("person1");
      const person2Select = document.getElementById("person2");

      // Clear existing options
      person1Select.innerHTML = "";
      person2Select.innerHTML = "";

      // Add options for all people in the family
      family.getNodes().forEach(node => {
        let option1 = document.createElement("option");
        option1.value = node.id;
        option1.textContent = node.name;
        person1Select.appendChild(option1);
        
        let option2 = document.createElement("option");
        option2.value = node.id;
        option2.textContent = node.name;
        person2Select.appendChild(option2);
      });

      // Show the popup
      popup.style.display = "block";
    }

    // Function to close the popup
    function closePopup() {
      document.getElementById("popup").style.display = "none";
    }

    // Function to compare two people and show the relationship
    function compare() {
      const person1Id = document.getElementById("person1").value;
      const person2Id = document.getElementById("person2").value;
      
      const person1 = family.getNode(person1Id);
      const person2 = family.getNode(person2Id);

      let relationship = `القرابة بين ${person1.name} و ${person2.name} هي...`;

      // For simplicity, just show a dummy relationship (can be enhanced)
      document.getElementById("relationship").textContent = relationship;
    }

    // Open popup on page load
    window.onload = openPopup;
  </script>
</body>
</html>
