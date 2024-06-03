![Screenshot%202024-05-28%2520160546.png](https://raw.githubusercontent.com/0rajnishk/sgbc_ppt_paradox/main/Screenshot%25202024-05-28%2520160546.png) 


# My Contribution to NeuroVoyager at the Sudha Gopalakrishnan Brain Centre, IIT Madras

For the first three months of my internship at the Sudha Gopalakrishnan Brain Centre, I worked as a full-stack developer. During this period, I contributed to both the frontend and backend development of NeuroVoyager. I used Django for the backend and Angular for the frontend to create a seamless and efficient brain research platform.

After my initial period, I transitioned to the AI/ML team. Here, I have been involved in implementing advanced features such as the Data Base Query Engine, NeuroGuardrails, and the Analytics Generator. These components are essential for interacting with our massive datasets, ensuring safe and relevant queries, and generating detailed analytics for various brain regions.

It's been a rewarding experience working here, especially with the weekly guidance and support from NVIDIA, which has been invaluable in advancing our projects.

## Event Presentation at IIT Madras Student Fest

Recently, we had the opportunity to present our work on NeuroVoyager at the IIT Madras Student Fest. During this event, we showcased the various features of NeuroVoyager to the students, highlighting the innovative tools and technologies we have developed for brain research.

We demonstrated how the platform facilitates high-resolution brain imaging, comprehensive data analysis, and advanced query handling. The response from the students was overwhelmingly positive, and it was a great chance to share our work and inspire future neuroscientists and engineers.

---

<center>
    <h1>NEUROVOYAGER</h1>
    <h2>Sudha Gopalakrishnan Brain Centre, IIT Madras </h2>
    <p>The Sudha Gopalakrishnan Brian Centre (SGBC) at IIT Madras has developed a brain research platform called Neuro Voyager. This platform focuses on high-resolution brain imaging at the cellular level, generating massive datasets for comprehensive brain analysis. In collaboration with NVIDIA, we have created a system capable of processing and visualizing data from hundreds of brains, facilitating groundbreaking research in neuroscience by making this data accessible globally</p>
</center>



## 1) DATA BASE QUERY ENGINE

<p>A simple easy to use query engine Where you can interact with the database in natural language</p>

There are four parameters.
 - species
 - organ
 - stain
 - age

![Screenshot%202024-05-28%20165234.png](https://raw.githubusercontent.com/0rajnishk/sgbc_ppt_paradox/main/Screenshot%25202024-05-28%2520165234.png)


```python
import requests
from IPython.display import Markdown, display

baseUrl = "https://apollo2.humanbrain.in/iipsrv/"
url = "https://apollo2.humanbrain.in/analytics/dbBrainQuery"

response = requests.post(url, json={"query": "fetal brain age less than 20 weeks NISL"})


if response.status_code == 200:
    response_data = response.json() 
    

    markdown_content = ""


    markdown_content += f"**Species**: {response_data['res']['species']}\n"
    markdown_content += f"**Organ**: {response_data['res']['organ']}\n"
    markdown_content += f"**Age Operator**: {response_data['res']['operator']} {response_data['res']['age']} weeks\n"


    markdown_content += "\n**Images:**\n\n"
    for item in response_data['msg']:
        image_url = baseUrl + item['pngPathLow']
        image_html = f'<img src="{image_url}" alt="{item["name"]}" style="max-width:200px">'
        details_html = f'<br>Name: {item["name"]}<br>Biosample: {item["biosample"]}<br>'
        markdown_content += f"{image_html}<br>{details_html}\n\n"


    display(Markdown(markdown_content))
else:
    print(f"Request failed with status code: {response.status_code}")
```

![dbquery](https://github.com/0rajnishk/sgbc_ppt_paradox/assets/96879734/98db91a1-8cf1-44eb-9c94-544e37086752)

**Species**: Human fetus
**Organ**: brain
**Age Operator**: < 20 weeks

**Images:**


# ==========================================================

## 2) NEUROGUARDRAILS

<p>Developed an agent who blocks queries that are unsafe and irrelevant to Neuroscience and brain anatomy using Nemoguardrails framework from NVIDIA and models from Meta
</p>


<table style="width:100%; border: 1px solid black; border-collapse: collapse;">
    <thead>
        <tr>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td style="border: 1px solid black; padding: 10px; text-align: center; font-size: 15px;"><b>Frameworks used</b></td>
            <td style="border: 1px solid black; padding: 10px; text-align: center; font-size: 15px;">NemoGuardrails, VLLM</td>
        </tr>
        <tr>
            <td style="border: 1px solid black; padding: 10px; text-align: center; font-size: 15px;"><b>Model used</b></td>
            <td style="border: 1px solid black; padding: 10px; text-align: center; font-size: 15px;">meta-llama/Meta-Llama-Guard-2-8B</td>
        </tr>
    </tbody>
</table>

![Screenshot%202024-05-28%20180851.png](https://raw.githubusercontent.com/0rajnishk/sgbc_ppt_paradox/main/Screenshot%25202024-05-28%2520180851.png)


```python
import requests
from utils import stream_text
url = "http://dgx3.humanbrain.in:1947/neuroguardrail"

response = requests.get(url, params={"query":"how to brain wash?"})

guard_eval = response.json()

stream_text(guard_eval['msg'])
```

    Our system specializes in answering questions and providing information about the brain and neuroscience. Regrettably, your query doesn't align with our focus.
    
    To offer accurate and relevant information, please ask about topics such as:
    
    - Neuroanatomy
    - Brain functions
    - Neuroscience research
    - Cognitive processes
    
    For questions within these areas, feel free to ask. For other topics, please consult relevant resources or services.


```python
import requests
from utils import stream_text
url = "http://dgx3.humanbrain.in:1947/neuroguardrail"

response = requests.get(url, params={"query":"what is neuroscience?"})

guard_eval = response.json()

stream_text(guard_eval['msg'])
```

    YES

# ==========================================================

## 3) ANALYTICS GENERATOR

<p>
When the explorer asks about a brain region or asks question based on function of the region, this feature returns the brain region name their area, perimeter and overall area it occupies for all the brains.
</p>

### ![Screenshot%202024-05-28%2520185431.png](https://raw.githubusercontent.com/0rajnishk/sgbc_ppt_paradox/main/Screenshot%25202024-05-28%2520185431.png)

<p>
Analytics can also be generated for a particular region of a particular brain id
</p>

![Screenshot%202024-05-28%20190131.png](https://raw.githubusercontent.com/0rajnishk/sgbc_ppt_paradox/main/Screenshot%25202024-05-28%2520190131.png)


```python
import requests
from IPython.display import display, Markdown
from utils import show_analytics, get_markdown

url = "http://dgx3.humanbrain.in:1947/get_stats"

# response = requests.get(url, params={"query":"describe the brain region responsible for memory"})
response = requests.get(url, params={"query":"describe the cerebellum?"})

if response.status_code == 200:
    data = response.json()
else:
    print(f"Failed to retrieve data: {response.status_code}")

data_list, overall_area = show_analytics(data)
tables_view = get_markdown(data_list, overall_area)
Markdown(tables_view)
```

    ## ANALYTICS GENERATOR
    
    
    Damage to the cerebellum can result in symptoms such as ataxia (loss of coordination and balance), tremors, slurred speech, and difficulty with fine motor skills. Conditions that can affect the cerebellum include stroke, multiple sclerosis, tumors, and genetic disorders.
    
    Treatment for cerebellar disorders typically involves physical therapy to improve coordination and balance, as well as medications to manage symptoms such as tremors. In some cases, surgery may be necessary to remove tumors or alleviate pressure on the cerebellum.
    
    Overall, the cerebellum is a crucial part of the brain that plays a vital role in movement and coordination, and damage to this region can have significant impacts on a person's quality of life.




<div style="display: inline-block; margin-right: 20px;"><div style='overflow-x:auto;'><table style='border-collapse: collapse; border: 2px solid black; font-family: Arial, sans-serif;'><thead><tr style='border-bottom: 2px solid black;'><th style='padding: 8px; background-color: #f2f2f2; border-right: 2px solid black;'>Biosample id</th><th style="border-right: 2px solid black;">Overall area in mm2</th></tr></thead><tbody><tr style="border-bottom: 1px solid #dddddd;"><td style="padding: 8px; border-right: 1px solid #dddddd;">142</td><td style="padding: 8px; border-right: 1px solid #dddddd;">5.97</td></tr><tr style="border-bottom: 1px solid #dddddd;"><td style="padding: 8px; border-right: 1px solid #dddddd;">222</td><td style="padding: 8px; border-right: 1px solid #dddddd;">0.24</td></tr></tbody></table></div></div><br/><br/><div style="display: inline-block;"><div style='overflow-x:auto;'><table style='border-collapse: collapse; border: 2px solid black; font-family: Arial, sans-serif;'><thead><tr style='border-bottom: 2px solid black;'><th style='padding: 8px; background-color: #f2f2f2; border-right: 2px solid black;'>id</th><th style="border-right: 2px solid black;">section</th><th style="border-right: 2px solid black;">region</th><th style="border-right: 2px solid black;">area</th><th style="border-right: 2px solid black;">perimeter</th></tr></thead><tbody><tr style="border-bottom: 1px solid #dddddd;"><td style="padding: 8px; border-right: 1px solid #dddddd;">222</td><td style="padding: 8px; border-right: 1px solid #dddddd;">1273</td><td style="padding: 8px; border-right: 1px solid #dddddd;">Cerebellum</td><td style="padding: 8px; border-right: 1px solid #dddddd;">4.03</td><td style="padding: 8px; border-right: 1px solid #dddddd;">8.09</td></tr><tr style="border-bottom: 1px solid #dddddd;"><td style="padding: 8px; border-right: 1px solid #dddddd;">142</td><td style="padding: 8px; border-right: 1px solid #dddddd;">889</td><td style="padding: 8px; border-right: 1px solid #dddddd;">Cerebellum</td><td style="padding: 8px; border-right: 1px solid #dddddd;">42.96</td><td style="padding: 8px; border-right: 1px solid #dddddd;">72.45</td></tr><tr style="border-bottom: 1px solid #dddddd;"><td style="padding: 8px; border-right: 1px solid #dddddd;">142</td><td style="padding: 8px; border-right: 1px solid #dddddd;">955</td><td style="padding: 8px; border-right: 1px solid #dddddd;">Cerebellum</td><td style="padding: 8px; border-right: 1px solid #dddddd;">56.55</td><td style="padding: 8px; border-right: 1px solid #dddddd;">103.17</td></tr></tbody></table></div></div>




```python
import requests
from IPython.display import display, Markdown
from utils import show_analytics, get_markdown

url = "http://dgx3.humanbrain.in:1947/get_stats"

# response = requests.get(url, params={"query":"describe the brain region responsible for memory"})
response = requests.get(url, params={"query":"describe the region responsible for memory?"})

if response.status_code == 200:
    data = response.json()
else:
    print(f"Failed to retrieve data: {response.status_code}")

data_list, overall_area = show_analytics(data)
tables_view = get_markdown(data_list, overall_area)
Markdown(tables_view)
```

    [1mANALYTICS GENERATOR[0m
    
    
    The Hippocampal formation is located in the medial temporal lobe and consists of the hippocampus, the dentate gyrus, and the subiculum. It is involved in the encoding, consolidation, and retrieval of memories. The hippocampus is particularly important for spatial memory and navigation, while the dentate gyrus is involved in pattern separation, which is the ability to distinguish between similar memories.
    
    Damage to the hippocampal formation can result in memory deficits, such as anterograde amnesia, which is the inability to form new memories, and retrograde amnesia, which is the loss of memories that were formed before the damage occurred. Patients with damage to this region may have difficulty remembering recent events or forming new memories, while older memories may remain intact.
    
    Overall, the hippocampal formation is a critical region for memory processing and plays a key role in our ability to remember past events, learn new information, and navigate our environment.


<div style="display: inline-block; margin-right: 20px;">
    <div style='overflow-x:auto;'>
        <table style='border-collapse: collapse; border: 2px solid black; font-family: Arial, sans-serif;'>
            <thead>
                <tr style='border-bottom: 2px solid black;'>
                    <th style='padding: 8px; background-color: #f2f2f2; border-right: 2px solid black;'>Biosample id
                    </th>
                    <th style="border-right: 2px solid black;">Overall area in mm2</th>
                </tr>
            </thead>
            <tbody>
                <tr style="border-bottom: 1px solid #dddddd;">
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">141</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">11.24</td>
                </tr>
                <tr style="border-bottom: 1px solid #dddddd;">
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">142</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">39.79</td>
                </tr>
                <tr style="border-bottom: 1px solid #dddddd;">
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">213</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">37.55</td>
                </tr>
                <tr style="border-bottom: 1px solid #dddddd;">
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">222</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">13.44</td>
                </tr>
                <tr style="border-bottom: 1px solid #dddddd;">
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">244</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">10.95</td>
                </tr>
            </tbody>
        </table>
    </div>
</div><br /><br />
<div style="display: inline-block;">
    <div style='overflow-x:auto;'>
        <table style='border-collapse: collapse; border: 2px solid black; font-family: Arial, sans-serif;'>
            <thead>
                <tr style='border-bottom: 2px solid black;'>
                    <th style='padding: 8px; background-color: #f2f2f2; border-right: 2px solid black;'>id</th>
                    <th style="border-right: 2px solid black;">section</th>
                    <th style="border-right: 2px solid black;">region</th>
                    <th style="border-right: 2px solid black;">area</th>
                    <th style="border-right: 2px solid black;">perimeter</th>
                </tr>
            </thead>
            <tbody>
                <tr style="border-bottom: 1px solid #dddddd;">
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">244</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">1078</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">Dentate gyrus</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">1.23</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">5.12</td>
                </tr>
                <tr style="border-bottom: 1px solid #dddddd;">
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">244</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">739</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">Dentate gyrus</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">1.38</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">5.72</td>
                </tr>
                <tr style="border-bottom: 1px solid #dddddd;">
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">244</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">739</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">Dentate gyrus</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">2.44</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">8.00</td>
                </tr>
                <tr style="border-bottom: 1px solid #dddddd;">
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">244</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">751</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">Dentate gyrus</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">2.02</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">6.80</td>
                </tr>
                <tr style="border-bottom: 1px solid #dddddd;">
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">141</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">1012</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">Hippocampal formation</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">1.97</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">7.16</td>
                </tr>
                <tr style="border-bottom: 1px solid #dddddd;">
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">141</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">1021</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">Hippocampal formation</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">3.59</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">11.96</td>
                </tr>
                <tr style="border-bottom: 1px solid #dddddd;">
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">141</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">1021</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">Hippocampal formation</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">1.54</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">6.57</td>
                </tr>
                <tr style="border-bottom: 1px solid #dddddd;">
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">141</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">991</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">Hippocampal formation</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">2.40</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">10.47</td>
                </tr>
                <tr style="border-bottom: 1px solid #dddddd;">
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">141</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">991</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">Hippocampal formation</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">1.48</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">6.26</td>
                </tr>
                <tr style="border-bottom: 1px solid #dddddd;">
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">141</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">1075</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">Dentate gyrus</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">2.22</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">13.47</td>
                </tr>
                <tr style="border-bottom: 1px solid #dddddd;">
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">141</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">1075</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">Hippocampal formation</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">1.54</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">7.17</td>
                </tr>
                <tr style="border-bottom: 1px solid #dddddd;">
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">141</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">1075</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">Hippocampal formation</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">1.96</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">8.50</td>
                </tr>
                <tr style="border-bottom: 1px solid #dddddd;">
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">141</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">367</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">Dentate gyrus</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">1.04</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">5.03</td>
                </tr>
                <tr style="border-bottom: 1px solid #dddddd;">
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">141</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">367</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">Dentate gyrus</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">0.43</td>
                    <td style="padding: 8px; border-right: 1px solid #dddddd;">2.51</td>
                </tr>
            </tbody>
        </table>
    </div>
</div>




```python
import requests
from IPython.display import display, Markdown
from utils import show_analytics, get_markdown

url = "http://dgx3.humanbrain.in:1947/get_stats"

# response = requests.get(url, params={"query":"describe the brain region responsible for memory"})
response = requests.get(url, params={"query":"describe the region responsible for attention?"})

if response.status_code == 200:
    data = response.json()
else:
    print(f"Failed to retrieve data: {response.status_code}")

data_list, overall_area = show_analytics(data)
tables_view = get_markdown(data_list, overall_area)
Markdown(tables_view)
```

    [1mANALYTICS GENERATOR[0m
    
    
    Additionally, the Cingulate cortex is connected to other regions of the brain involved in attention, such as the prefrontal cortex and the parietal cortex. These connections allow for the coordination of attentional processes and the integration of information from different sources.
    
    Damage or dysfunction in the Cingulate cortex can lead to difficulties in maintaining attention, regulating emotions, and making decisions. Conditions such as attention deficit hyperactivity disorder (ADHD), depression, and anxiety disorders have been linked to abnormalities in the Cingulate cortex.
    
    Overall, the Cingulate cortex plays a crucial role in attentional processes and is essential for our ability to focus, make decisions, and regulate our emotions.




<div style="display: inline-block; margin-right: 20px;"><div style='overflow-x:auto;'><table style='border-collapse: collapse; border: 2px solid black; font-family: Arial, sans-serif;'><thead><tr style='border-bottom: 2px solid black;'><th style='padding: 8px; background-color: #f2f2f2; border-right: 2px solid black;'>Biosample id</th><th style="border-right: 2px solid black;">Overall area in mm2</th></tr></thead><tbody><tr style="border-bottom: 1px solid #dddddd;"><td style="padding: 8px; border-right: 1px solid #dddddd;">213</td><td style="padding: 8px; border-right: 1px solid #dddddd;">0.02</td></tr></tbody></table></div></div><br/><br/><div style="display: inline-block;"><div style='overflow-x:auto;'><table style='border-collapse: collapse; border: 2px solid black; font-family: Arial, sans-serif;'><thead><tr style='border-bottom: 2px solid black;'><th style='padding: 8px; background-color: #f2f2f2; border-right: 2px solid black;'>id</th><th style="border-right: 2px solid black;">section</th><th style="border-right: 2px solid black;">region</th><th style="border-right: 2px solid black;">area</th><th style="border-right: 2px solid black;">perimeter</th></tr></thead><tbody><tr style="border-bottom: 1px solid #dddddd;"><td style="padding: 8px; border-right: 1px solid #dddddd;">213</td><td style="padding: 8px; border-right: 1px solid #dddddd;">1060</td><td style="padding: 8px; border-right: 1px solid #dddddd;">Cingulate cortex</td><td style="padding: 8px; border-right: 1px solid #dddddd;">0.28</td><td style="padding: 8px; border-right: 1px solid #dddddd;">3.29</td></tr><tr style="border-bottom: 1px solid #dddddd;"><td style="padding: 8px; border-right: 1px solid #dddddd;">213</td><td style="padding: 8px; border-right: 1px solid #dddddd;">1060</td><td style="padding: 8px; border-right: 1px solid #dddddd;">Cingulate cortex</td><td style="padding: 8px; border-right: 1px solid #dddddd;">0.13</td><td style="padding: 8px; border-right: 1px solid #dddddd;">3.80</td></tr></tbody></table></div></div>



# ==========================================================

## 4) Biosample Info

The simple yet powerful i-feature can be used to know the histology and digitalization details of the biosample which is live from database.

![image.png](image.png)


```python
import requests
from IPython.display import display, Markdown
from utils import show_analytics, get_markdown

url = "http://dgx3.humanbrain.in:1947/api/v1/describe"

# response = requests.get(url, params={"query":"describe the brain region responsible for memory"})
response = requests.get(url, params={"biosample_id":"242"})
data = None
if response.status_code == 200:
    data = response.json()
else:
    print(f"Failed to retrieve data: {response.status_code}")
    
Markdown(data['response'])
```




<h5>Histology</h5><h6>1. Subject</h6><ul><li>The specimen referred to as Human Brain 1 NIMHANS is a biological sample aged 50.0 years.</li><li>The gender of the specimen is identified as male.</li><li>The specimen consists of sample of the Brain of the species Human.</li></ul><h6>2. Biosample</h6><ul><li>The specimen acquisition was external from  laboratory NIMMHANS.</li><li>The sample was created on 2023-06-14 at 06:40.</li></ul><h5>Digitalization</h5><ul><li>Block Face Image count: 1902</li><li>MRI Image count: 0</li><li>List of stains: None.</li><li>No. of sections digitized<br>Quality Check (QC) details<ol><li>NISSL - Total QC Passed: 0.</li><li>HEOS - Total QC Passed: 0.</li></ol></li></ul>




```python

```


```python

```
